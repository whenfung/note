---
title: merge
---

将多个 `Observables` 合并成一个。

通过使用 **merge** 操作符你可以将多个 `Observables` 合并成一个，当某一个 `Observable` 发出一个元素时，他就将这个元素发出。

## 演示

```swift
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    @IBOutlet weak var usernameTextField: UITextField!
    @IBOutlet weak var passwordTextField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        let usernameDriver = usernameTextField.rx.controlEvent(.editingChanged).asDriver()
        let passwordDriver = passwordTextField.rx.controlEvent(.editingChanged).asDriver()
        
        Driver.merge(usernameDriver, passwordDriver)
            .drive(onNext: { () in
                print("输入框内容发生了改变")
            }).disposed(by: disposeBag)
        
    }
}
```

## 场景

- 不同手势响应相同事件

当不同的事件需要相同的响应的时候，可以把事件合并到一个序列里。

例如点击事件和下滑事件都会触发键盘隐藏，那么就可以将点击事件和下滑事件合并为一个序列。

```swift
Driver.merge(
	tap.rx.event.map { _ in () }.asDriver(onErrorJustReturn: ()),
  swipe.rx.event.map { _ in () }.asDriver(onErrorJustReturn: ())
)
```

- 不同输入框响应相同事件

在输入账号密码之后，我们点击登录，如果失败则输入框会标红，如果有多个输入框，我们希望在任意一个输入框中编辑，标红都会立即消失，这时候就可以使用 `merge` 操作符。

```swift
import RxSwift
import RxCocoa

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    @IBOutlet weak var usernameTextField: UITextField!
    @IBOutlet weak var passwordTextField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        usernameTextField.text = "你好"
        
        let usernameDriver = usernameTextField.rx.text.orEmpty.asDriver()
        let passwordDriver = passwordTextField.rx.text.orEmpty.asDriver()
        
        let textDrivers = Driver.merge(usernameDriver, passwordDriver)
        
        textDrivers.drive(onNext: { text in
            print(text)
            }).disposed(by: disposeBag)
    }
}
```

