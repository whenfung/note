---
title: withLatestFrom
---

将两个 `Observables` 最新的元素通过一个函数组合起来，当第一个 `Observable` 发出一个元素，就将组合后的元素发送出来

`withLatestFrom` 操作符将两个 `Observables` 中最新的元素通过一个函数组合起来，然后将这个组合的结果发出来。当第一个 `Observable` 发出一个元素时，就立即取出第二个 `Observable` 中最新的元素，通过一个组合函数将两个最新的元素合并后发送出去。

**注意**：如果第二个 Observable 是空序列，那么什么都不会发生。

## 演示

当第一个 `Observable` 发出一个元素时，就立即取出第二个 `Observable` 中最新的元素，然后把第二个 `Observable` 中最新的元素发送出去。

```swift
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    override func viewDidLoad() {
        super.viewDidLoad()

        let disposeBag = DisposeBag()
        let firstSubject = PublishSubject<String>()
        let secondSubject = PublishSubject<String>()

        firstSubject
             .withLatestFrom(secondSubject)
             .subscribe(onNext: { print($0) })
             .disposed(by: disposeBag)

        firstSubject.onNext("🅰️")
        firstSubject.onNext("🅱️")
        secondSubject.onNext("1")
        secondSubject.onNext("2")
        firstSubject.onNext("🆎")
    }
}
```

输出：

```bash
2
```

解析：由于 `firstSubject` 发送事件时，`secondSubject` 对应的序列为空，所以什么都不会发生。

紧接着 `secondSubject` 塞入了两个事件 1 和 2。

此时 `firstSubject` 再次发送事件，触发了订阅，输出 2。