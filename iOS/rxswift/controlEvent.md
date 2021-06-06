---
title: controlEvent
---

**ControlEvent** 专门用于描述 **UI** 控件所产生的事件，它具有以下特征：

- 不会产生 `error` 事件
- 一定在 `MainScheduler` 订阅（主线程订阅）
- 一定在 `MainScheduler` 监听（主线程监听）
- 共享附加作用

## 演示

```swift
usernameTextField.rx.controlEvent(.editingChanged).asDriver()
    .drive(onNext: { () in
        print("内容发生了改变")
    }).disposed(by: disposeBag)
```

## 场景

我们有时候可以根据输入框的内容是否改变来做一些事情。

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

        usernameTextField.text = "你好"
        
        let usernameDriver = usernameTextField.rx.controlEvent(.editingChanged).asDriver()
        let passwordDriver = passwordTextField.rx.controlEvent(.editingChanged).asDriver()
        
        let textDrivers = Driver.merge(usernameDriver, passwordDriver)
        
        textDrivers.drive(onNext: { text in
            print(text)
            }).disposed(by: disposeBag)
    }
}
```

上述例子，只要使用键盘改变输入框的值，那么就会触发事件，但是不会触发初始值，也就是不会输出 “你好”。

而下面的例子会输出 “你好”：

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

但是，上面的例子同样会带来问题，每当我们选中或取消选中输入框的时候，都会发出事件。所以我们可以做一些修改，使得输入框内容发生改变才发出事件：

```swift
import UIKit
import RxSwift
import RxCocoa

extension String: Error {}

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    @IBOutlet weak var usernameTextField: UITextField!
    @IBOutlet weak var codeTextField: UITextField!
    @IBOutlet weak var passwordTextField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        let usernameDriver = usernameTextField.rx.text.orEmpty.asDriver()
        let codeDriver = codeTextField.rx.text.orEmpty.asDriver()
        let passwordDriver = passwordTextField.rx.text.orEmpty.asDriver()
        
        let textDrivers = Driver.combineLatest(usernameDriver, codeDriver, passwordDriver)
        
        textDrivers.distinctUntilChanged(==)
            .drive(onNext: { text in
                print("text: ", text)
            }).disposed(by: disposeBag)
        
    }
}
```

## 受代理影响

有时候我们需要监听回车键的响应，我们可以在代理中做一些判断，使得回车在某些情况有效，例如下面的订阅是不会有输出的。

```swift
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {
    
    let disposeBag = DisposeBag()
    @IBOutlet weak var textField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        textField.rx.controlEvent(.editingDidEndOnExit)
            .subscribe { _ in
                print("输入框回车了")
            }.disposed(by: disposeBag)
        
    }
}

extension ViewController: UITextFieldDelegate {
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        return false
    }
}
```

