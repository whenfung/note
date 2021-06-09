---
title: signal
---

**Signal** 和 Driver 相似，唯一的区别是，Driver **会**对新观察者回放（重新发送）上一个元素，而 **Signal** **不会**对新观察者回放上一个元素。

- 使用 driver 监听事件如下，第二次订阅时新观察者**接收到**第一次订阅时发出的事件。

```swift
import UIKit
import RxSwift
import RxCocoa

extension String: Error {}

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    @IBOutlet weak var button: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        let event: Driver<Void> = button.rx.tap.asDriver()
        event.drive(onNext: {
            print("早上好")
            event.drive(onNext: {
                print("晚上好")
            }).disposed(by: self.disposeBag)
        }).disposed(by: disposeBag)

    }
}
```

第二次点击的输出为：

```swift
早上好
晚上好
```

- 使用 signal 监听事件如下，第二次订阅时新观察者**接收不到**第一次订阅时发出的事件。

```swift
import UIKit
import RxSwift
import RxCocoa

extension String: Error {}

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    @IBOutlet weak var button: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        let event : Signal<Void> = button.rx.tap.asSignal()
        event.emit(onNext: {
            print("早上好")
            event.emit(onNext: {
                print("晚上好")
            }).disposed(by: self.disposeBag)
        }).disposed(by: disposeBag)

    }
}
```

第一次点击的输出为：

```
早上好
```

