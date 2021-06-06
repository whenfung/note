---
title: map
---

通过一个转换函数，将 `Observable` 的每个元素转换一遍。

**map** 操作符将源 `Observable` 的每个元素应用你提供的转换方法，然后返回含有转换结果的 `Observable`。

## 演示

- 修改序列中的每个事件的值。

```swift
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    override func viewDidLoad() {
        super.viewDidLoad()

        Observable.of(1, 2, 3)
            .map { $0 * 10 }
            .subscribe(onNext: { print($0) })
            .disposed(by: disposeBag)
        
    }
}
```

输出结果为：

```swift
10
20
30
```

## 场景

- 如果序列不是 Observable，那么你可以通过 map 实现序列的转换。
- 如果对序列的元素做一些预处理，可以使用 `map` 方法。

