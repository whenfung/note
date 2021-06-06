---
title: distinctUntilChanged
---

阻止 `Observable` 发出相同的元素。

`distinctUntilChanged` 操作符将阻止 `Observable` 发出相同的元素。如果后一个元素和前一个元素是相同的，那么这个元素将不会被发出来。如果后一个元素和前一个元素不相同，那么这个元素才会被发出来。

例如：

```swift
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        Observable.of("1", "2", "3", "3", "1", "2", "2")
            .distinctUntilChanged()
            .subscribe(onNext: { print($0) })
            .disposed(by: disposeBag)
    }
}
```

输出结果如下：

```swift
1
2
3
1
2
```

## 使用场景

- 如果相同的事件作出的反应是一样的，那么重复做相同的事情是耗资源的，所以我们可以干脆不发出相同的元素。
- 当输入框内容发生改变时才发出事件

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



