---
title: delay
---

将 `Observable` 的每一个元素拖延一段时间后发出。

`delay` 操作符将修改一个 `Observable`，它会将 `Observable` 的所有元素都拖延一段设定好的时间， 然后才将它们发送出来。

例如在登录场景中，我们可能会弹框 3 秒提示用户登录成功，然后再进行页面跳转，这时候就需要 delay 延迟页面跳转时机。

例如下面例子，每次 textField 的文本改变后，都会先延迟 3 秒中再将文本赋给 label。

```swift
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    @IBOutlet weak var label: UILabel!
    @IBOutlet weak var textField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let delay: RxTimeInterval = .seconds(3)
        
        _ = textField.rx.text.orEmpty.asDriver()
            .flatMap { input -> Driver<String> in
                return Driver.just(input).delay(delay)
        }.drive(onNext: { [weak self] input in
            print("延迟了三秒哦")
            self?.label.text = input
        })
    }
}
```

其中 just 创建 `Observable` 发出唯一的一个元素。