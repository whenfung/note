---
title: skip
---

跳过 `Observable` 中头 n 个元素

**skip** 操作符可以让你跳过 `Observable` 中头 n 个元素，只关注后面的元素。

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
            .skip(2)
            .subscribe(onNext: { print($0) })
            .disposed(by: disposeBag)
    }
}
```

结果如下：

```bash
3
4
5
6
```

