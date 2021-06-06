---
title: flatMap
---

将 `Observable` 的元素转换成其他的 `Observable`，然后将这些 `Observables` 合并。

flatMap 操作符将源 `Observable` 的每一个元素应用一个转换方法，将他们转换成 `Observables`。 然后将这些 `Observables` 的元素合并之后再发送出来。

这个操作符是非常有用的，例如，当 `Observable` 的元素本身拥有其他的 `Observable` 时，你可以将所有**子** `Observables` 的元素发送出来。

## 演示

- 合并功能

```swift
import UIKit
import RxSwift
import RxCocoa

struct Player {
    init(score: Int) {
        self.score = BehaviorSubject(value: score)
    }

    let score: BehaviorSubject<Int>
}

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    override func viewDidLoad() {
        super.viewDidLoad()

        let 弟弟 = Player(score: 80)
        let 哥哥 = Player(score: 90)
        
        let player = BehaviorSubject(value: 弟弟)
        
        player.asObservable()
            .flatMap { $0.score.asObservable() } // Change flatMap to flatMapLatest and observe change in printed output
            .subscribe(onNext: { print($0) })
            .disposed(by: disposeBag)
        
        弟弟.score.onNext(85)
        
        player.onNext(哥哥)
        
        弟弟.score.onNext(95) // Will be printed when using flatMap, but will not be printed when using flatMapLatest
        
        哥哥.score.onNext(100)
        
    }
}
```

这里的 `flatMap` 将所有的 score 合并为一个序列了。

结果为：

```swift
80
85
90
95
100
```

- 延迟执行序列

在 flatMap 中将所有的事件延后 2 秒执行，这样就能达到延迟执行序列的效果。

```swift
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()

    @IBOutlet weak var textField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        textField.rx.text.orEmpty.asDriver().flatMap { text -> Driver<String> in
            return Driver.just(text).delay(.seconds(2))
        }.drive(onNext: { text in
            print(text)
            }).disposed(by: disposeBag)
    }
}
```

