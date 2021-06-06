---
title: debug
---

打印所有的订阅，事件以及销毁信息。

```swift
import UIKit
import RxSwift
import RxCocoa

extension String: Error {}

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    
    override func viewDidLoad() {
        super.viewDidLoad()

        let sequence = Observable<String>.create { observer in
            observer.onNext("apple")
            observer.onNext("pear")
            observer.onCompleted()
            return Disposables.create()
        }

        sequence
            .debug("Fruit")
            .subscribe()
            .disposed(by: disposeBag)
    }
}
```

打印信息如下：

```bash
2020-09-16 11:43:39.778: Fruit -> subscribed
2020-09-16 11:43:39.780: Fruit -> Event next(apple)
2020-09-16 11:43:39.780: Fruit -> Event next(pear)
2020-09-16 11:43:39.781: Fruit -> Event completed
2020-09-16 11:43:39.781: Fruit -> isDisposed
```

