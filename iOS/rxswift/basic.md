---
title: basic
---

函数式 + 异步式 = 响应式，RxSwift 是函数响应式编程的一个开源库。

GitHub 的 ReactiveX 组织开发、维护。

Rx.net、RxJava、RxJS

数据/事件流和异步任务能够更方便地序列化处理（按顺序处理呗）

## 观察者模式

一旦发现观察对象发生改变，就立即作出响应。如果观察者成为一条链，则成了一个序列。

注意 ：观察对象必须在订阅之前就存在，例如下面做法就是错的

```swift
var button: UIButton?
button?.rx.tap.subscribe({ _ in
    print("订阅有效")
}).disposed(by: disposeBag)
```

- KVO、通知、代理。重要的设计模式

- 监听婴儿的哭声

  宝宝是被观察者，父母是观察者，也可称为订阅者，叫声是一个事件。一旦事件发生，通知订阅者，订阅者可立即作出反应。

- iOS 开发中很多事件监听都是观察者设计模式

## RxSwift 做了什么

- RxSwift 把每一个操作都看成一个事件，所有事件源可以放到一个管道中，也就是 sequence。

  - TextField 的文本改变
  - 按钮被点击
  - 网络请求结束

  管道中的事件一定会按顺序发生，我们就可以按顺序处理事件。

- 例如 TextField，当我们改变里面的文本的时候，这个 TextField 就会不断地发出事件，事件全部放到 sequence 中，我们只要监听这个 sequence，就可以按顺序处理 sequence 中的每一个事件。

- 同样，Button 也是一个 sequence，每点击一次就流出一个事件。

- 序列包括正常结束序列、异常结束序列、无限序列（点击）

## Observable & Observer

observer 是观察者，observable 是事件源。

报社 ➡️ 报亭 ➡️ 订阅者，顺序如下：

- 订阅者到报亭订阅
- 报社把最新报纸发给报亭
- 报亭发给订阅者

因此，报社就是事件源头，报亭就是观察者，顶包的人就是订阅者。

类比，按钮点击就是事件源头，观察点击事件，处理点击事件。

类比，UITextField 文字改变是事件源头、观察文字改变事件，处理文字改变后的其他事件。

类比，对象属性改变（KVO），监听改变，处理属性改变。

类比，UITableView 数据源改变，数据改变，重新处理数据。

## RxSwift 初体验

RxSwift 包括一些原始的数据流；RxCocoa 封装控件相关的响应，是 RxSwift 的拓展，常用的控件可以直接使用。这两个库相辅相成。

使用 cocopods 集成到项目中最简单，

- 也可以编译成静态库放到项目中，但是这样就看不到源码了，而且要在 build 中配置库。
- 如果直接将源码 RxSwift 和 RxCocoa 文件夹放项目中，大概率会出现问题。

### 1. 按钮

如果使用故事板来完成按钮点击事件，代码会被分散为三部分

- 控件变量
- target-action

随着代码量变多（1000 行），代码都找不到，修改代码就太不方便了。

---

如果用 RxSwift 来做这件事，按照 ：数据源 ➡️ 订阅点击事件 ➡️ 作出响应，样例如下

```swift
button.rx.tap
    .subscribe { (event: Event<Void>) in
        print("按钮 1 发生了点击")
}
```

- 第一行就是事件源（点击）
- 第二行就是订阅点击事件
- 第三行就是对序列的每个事件作出相应的响应

上述代码会发生一个警告，这是因为上述的订阅会返回一个值，这个值理应需要使用。补充如下：

```swift
button.rx.tap
    .subscribe { (event: Event<Void>) in
        print("按钮 1 发生了点击")
}.disposed(by: disposeBag)
```

其中，`disposeBag` 是一个类常量，如下：

```swift
private let disposeBag = DisposeBag()
```

具体 `DisposeBag` 的使用会后面介绍。

### 2. UITextField

接下来我们监听 UITextField 的文字改变，传统方式需要添加代理，然后实现代理方法。最麻烦的是：

- 对于多个 UITextField，最后都代理到相同的方法，然后需要加 `if` 判断，太麻烦了。

所以不只是有 button 监听事件的缺点，如果一个页面有很多的 UITextField，就需要做很多的判断，这傻逼玩意。

```swift
// viewDidLoad 
textField.delegate = self

extension ViewController: UITextFieldDelegate {
    func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
        print("输入框内容发生改变")
        print(textField.text!)
        return true
    }
}
```

而且输出的内容不包括当前输入的字符。

---

RxSwift 订阅文字改变的方式如下：

```swift
textField.rx.text
    .subscribe { (event: Event<String?>) in
        // 解包两次
        print(event.element!!)
}.disposed(by: disposeBag)
```

- 第一行是事件源
- 第二行订阅该事件
- 第三行作出相应处理

你可以发现需要解包两次太麻烦了，我们可以使用其他更简单的方法，如下：

```swift
textField.rx.text
    .subscribe(onNext: { (str: String?) in
        print(str!)
    }).disposed(by: disposeBag)
```

其实这就是观察者模式，最常见的就是当登录注册页面中，网络请求失败后输入框会发生标红，此时用户再次编辑文本后，我们就需要变回原来的颜色。

同一个事件源可以订阅多次，通俗点说，相同的事件 `subscribe` 多次不会报错。

### 3. 数据绑定

假如有那么一个场景，一个 label 的 text 是跟随一个 TextField 的文本变的，通过传统的监听，然后赋值就可以做到，例如：

```swift
textField.rx.text
    .subscribe(onNext: { (str: String?) in
        self.label.text = str
    }).disposed(by: disposeBag)
```

但是这不是 RxSwift 的合理做法，正确做法如下：

```swift
textField.rx.text
    .bind(to: label.rx.text)
.disposed(by: disposeBag)
```

相当于把 textField 的文本和 label 的文本进行绑定，我尝试反过来用 label 的文本绑定，但是没有相应的方法。

### 4. KVO 传统方式

```swift
// KVO
textField.addObserver(self, forKeyPath: "text", options: .new, context: nil)

override func observeValue(forKeyPath keyPath: String?, of object: Any?, change: [NSKeyValueChangeKey : Any]?, context: UnsafeMutableRawPointer?) {
    print((change?[.newKey])!)
}
```

可以发现传统的 KVO 监听最后的实现方法都是统一的，因此我们需要在 `observeValue` 对所有的键进行判断。例如

```swift
if keyPath == "text"
```

但是上述的判断还是有问题，如果有两个 TextField 的都是观察 `text` 属性，那么 keyPath 就没用了。这时候需要使用 object 来判断：

```swift
if textField.isEqual(object) {
    if keyPath == "text" {
        print((change?[.newKey])!)
    }
}
```

这只是一个类的一个属性，如果属性太多了，呵呵，那可就太麻烦了。

---

我们看看 RxSwift 的方式：

```swift
// KVO
textField.rx.observe(String.self, "text")
    .subscribe(onNext: { (string: String?) in
        print(string!)
        self.btn1.titleLabel?.text = string
    }).disposed(by: disposeBag)
```

监听 text 属性的变化 ➡️ 订阅该监听 ➡️ 作出响应

### 5. UIScrollView 的滚动

滚动的传统方式，首先要设置代理，然后实现代理，如下：

```swift
scrollView.delegate = self

extension ViewController: UIScrollViewDelegate {
    func scrollViewDidScroll(_ scrollView: UIScrollView) {
        print(scrollView.contentOffset)
    }
}
```

可以看出，代理模式的代码内聚性特别差。

---

我们看看 RxSwift 的实现方式：

```swift
scrollView.rx.contentOffset
    .subscribe(onNext: { (point: CGPoint) in
        print(point)
    }).disposed(by: disposeBag)
```

步骤还是很固定：

- 第一行找出事件源
- 第二行订阅
- 第三行对事件源的改变作出反应





