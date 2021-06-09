---
title: Driver
---

Driver 特征是序列实际在开发中专门用于

- 不会产生 error 事件
- 一定在主线程监听（MainScheduler）
- 共享状态变化（shareReplayLatestWhileConnected）

由于 drive 方法只能被 Driver 调用。这意味着，如果代码存在 drive，那么这个序列不会产生错误事件并且一定在主线程监听。这样我们就可以安全的绑定 UI 元素。

```swift
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    @IBOutlet weak var label: UILabel!
    @IBOutlet weak var textField: UITextField!
    @IBOutlet weak var button: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let result = textField.rx.text.orEmpty
            .asDriver()
            .flatMap {
                return self.dealData(inputText: $0)
                    .asDriver(onErrorJustReturn: "检测到了错误事件")
            }
        
        // 第一次订阅
        result.map { "按钮显示的标题长度：\(($0).count)" }
            .drive(label.rx.text)
            .disposed(by: disposeBag)
        
        // 第二次订阅
        result.map { $0 }
            .drive(button.rx.title())
            .disposed(by: disposeBag)
    }
 
    // 模拟网络请求代码
    func dealData(inputText: String) -> Observable<String> {
        print("请求网络了 \(Thread.current)")
        return Observable<String>.create({ (observer) -> Disposable in
            // 模拟发出错误
            if inputText == "1234" {
                observer.onError(NSError(domain: "deng.com", code: 10086, userInfo: nil))
            }
            
            // 异步发送信号
            DispatchQueue.global().async {
                print("发送之前看看：\(Thread.current)")
                observer.onNext("已经输入：" + inputText)
                observer.onCompleted()
            }
            return Disposables.create()
        })
    }
}
```

输入 1 时，打印结果如下：

```bash
请求网络了 <NSThread: 0x600003ce00c0>{number = 1, name = main}
发送之前看看：<NSThread: 0x600003cb3380>{number = 3, name = (null)}
```

输入 1234 时，打印结果如下：

```bash
请求网络了 <NSThread: 0x600003ce00c0>{number = 1, name = main}
发送之前看看：<NSThread: 0x600003cb3380>{number = 3, name = (null)}
```

✅ 同时我们可以发现，故事板的 UI 渲染正常。

## observable

为什么要用 Driver 呢，因为如果你用 Observable 的话会做很多操作，如下：

```swift
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    @IBOutlet weak var label: UILabel!
    @IBOutlet weak var textField: UITextField!
    @IBOutlet weak var button: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 序列的创建
        let result = textField.rx.text.skip(1).flatMap { [weak self](input) -> Observable<String> in
             return (self?.dealData(inputText: input ?? ""))!
        }
        
        // 第一次订阅
        result.subscribe { (even) in
            print(even)
            print(Thread.current)
        }.disposed(by: disposeBag)
        
        // 第二次订阅
        result.subscribe { (even) in
            print(even)
            print(Thread.current)
        }.disposed(by: disposeBag)
    }
 
    // 模拟网络请求代码
    func dealData(inputText: String) -> Observable<String> {
        print("请求网络了 \(Thread.current)")
        return Observable<String>.create({ (observer) -> Disposable in
            // 模拟发出错误
            if inputText == "1234" {
                observer.onError(NSError(domain: "deng.com", code: 10086, userInfo: nil))
            }
            
            // 异步发送信号
            DispatchQueue.global().async {
                print("发送之前看看：\(Thread.current)")
                observer.onNext("已经输入：" + inputText)
                observer.onCompleted()
            }
            return Disposables.create()
        })
    }
}
```

运行，输入 2 时打印结果如下：

```bash
请求网络了 <NSThread: 0x600001940240>{number = 1, name = main}
发送之前看看：<NSThread: 0x600001930500>{number = 5, name = (null)}
next(已经输入：2)
<NSThread: 0x600001930500>{number = 5, name = (null)}
请求网络了 <NSThread: 0x600001940240>{number = 1, name = main}
发送之前看看：<NSThread: 0x600001930500>{number = 5, name = (null)}
next(已经输入：2)
<NSThread: 0x600001930500>{number = 5, name = (null)}
```

打印结果中看出，订阅两次时，我们会请求两次网络。请求多次网络解决办法：

- 观察时增加共享指定

```swift
// 序列的创建
let result = textField.rx.text.skip(1).flatMap { [weak self](input) -> Observable<String> in
     return (self?.dealData(inputText: input ?? ""))!
}.share(replay: 1, scope: .whileConnected)
```

再运行得到：

```bash
请求网络了 <NSThread: 0x600001e90240>{number = 1, name = main}
发送之前看看：<NSThread: 0x600001ed1cc0>{number = 5, name = (null)}
next(已经输入：1)
<NSThread: 0x600001ed1cc0>{number = 5, name = (null)}
next(已经输入：1)
<NSThread: 0x600001ed1cc0>{number = 5, name = (null)}
```

看到订阅两次但是只执行一次监听方法。但是我们仍然看到接受到事件时是处于子线程，意味着不能更新UI事件。继续解决：

- 响应异步解决办法 : 指定序列监听执行线程

```bash
// 序列的创建
let result = textField.rx.text.skip(1).flatMap { [weak self](input) -> Observable<String> in
     return (self?.dealData(inputText: input ?? ""))!
        .observeOn(MainScheduler.instance)
}.share(replay: 1, scope: .whileConnected)
```

打印结果：

```bash
请求网络了 <NSThread: 0x600001dcc0c0>{number = 1, name = main}
发送之前看看：<NSThread: 0x600001df5f40>{number = 5, name = (null)}
next(已经输入：1)
<NSThread: 0x600001dcc0c0>{number = 1, name = main}
next(已经输入：1)
<NSThread: 0x600001dcc0c0>{number = 1, name = main}
```

我们看到当输入内容为 1234 时，打印结果如下：

```bash
请求网络了 <NSThread: 0x600003024d40>{number = 1, name = main}
error(Error Domain=deng.com Code=10086 "(null)")
<NSThread: 0x600003024d40>{number = 1, name = main}
error(Error Domain=deng.com Code=10086 "(null)")
<NSThread: 0x600003024d40>{number = 1, name = main}
发送之前看看：<NSThread: 0x60000305e0c0>{number = 6, name = (null)}
```

此时所有订阅都会被取消，也就是不会再有反应了。

- 增加错误 handle

```swift
// 序列的创建
let result = textField.rx.text.skip(1).flatMap { [weak self](input) -> Observable<String> in
     return (self?.dealData(inputText: input ?? ""))!
        .observeOn(MainScheduler.instance)
    .catchErrorJustReturn("检测到了错误事件。")
}.share(replay: 1, scope: .whileConnected)
```

再次输入 1234，打印得：

```bash
请求网络了 <NSThread: 0x60000236c480>{number = 1, name = main}
next(检测到了错误事件。)
<NSThread: 0x60000236c480>{number = 1, name = main}
next(检测到了错误事件。)
<NSThread: 0x60000236c480>{number = 1, name = main}
发送之前看看：<NSThread: 0x6000023e4a40>{number = 9, name = (null)}
```

- 总结：我们为了处理

  - 请求多次网络解决办法

  - 响应异步解决办法
  - 增加错误 handle

  需要分别额外增加三步处理，可是在实际项目中，往往不会使用这种方式。而是用 Driver 来处理。