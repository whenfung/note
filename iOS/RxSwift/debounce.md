---
title: debounce
---

过滤掉高频产生的元素

`debounce` 操作符将发出这种元素，在 `Observable` 产生这种元素后，一段时间内没有新元素产生。

例如：

```swift
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    override func viewDidLoad() {
        super.viewDidLoad()

        Observable.of("1", "2", "3", "4", "5", "6")
            .debounce(.seconds(2), scheduler: MainScheduler.instance)
            .subscribe(onNext: { print("element: ", $0) })
            .disposed(by: disposeBag)
    }
}
```

