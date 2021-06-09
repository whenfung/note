---
title: catchError
---

从一个错误事件中恢复，将错误事件替换成一个备选序列。

**catchError** 操作符将会拦截一个 `error` 事件，将它替换成其他的元素或者一组元素，然后传递给观察者。这样可以使得 `Observable` 正常结束，或者根本都不需要结束。

## 演示

```swift
import UIKit
import RxSwift
import RxCocoa

extension String: Error {}

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    override func viewDidLoad() {
        super.viewDidLoad()

        let sequenceThatFails = PublishSubject<String>()
        let recoverySequence = PublishSubject<String>()

        sequenceThatFails
            .catchError {
                print("Error:", $0)
                return recoverySequence
            }
            .subscribe { print($0) }
            .disposed(by: disposeBag)

        sequenceThatFails.onNext("😬")
        sequenceThatFails.onNext("😨")
        sequenceThatFails.onNext("😡")
        sequenceThatFails.onNext("🔴")
        sequenceThatFails.onError("你好")

        recoverySequence.onNext("😊")

    }
}
```



