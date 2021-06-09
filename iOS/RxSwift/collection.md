---
title: RxSwift collection
---

通常来说，一个序列如果发出了 `error` 或者 `completed` 事件，那么所有内部资源都会被释放。

我们看下面的错误例子：

```swift
{
	_ = subject.subscribe({ (event: Event<String>) in
    print(event)
  })
}
```

在我们之前的印象中，在作用域内的变量会在跳出作用域后释放。

但是当 subject 被订阅之后，订阅的内部会对 subject 做一个强引用。

```swift
// disposable 手动释放
let subject = BehaviorSubject(value: "a")
subject.subscribe({ (event: Event<String>) in
    print(event)
    }).dispose()

subject.onNext("b")
```

使用 `dispose()` 进行立即释放，但是这样的话 `subject` 就不能收到订阅之后的更新了，也就是说：收不到 b 的事件。（用着用着就没了，太惨了）

但是我们一般是让类里面的变量跟着类的销毁而销毁。

## 1. Disposable

可以使用 Disposable 手动回收资源。

## 2. DisposeBag

可以用清除包 DisposeBag 管理订阅的生命周期。在合适的时候把 DisposeBag 扔掉即可。

```swift
class ViewController: UIViewController {

    private let disposeBag = DisposeBag()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // disposable 手动释放
        let subject = BehaviorSubject(value: "a")
        subject.subscribe({ (event: Event<String>) in
            print(event)
            }).disposed(by: disposeBag)
        
        subject.onNext("b")
    }
    
}
```

当类被销毁的时候，disposeBag 就会被立即销毁，此时它会调用所有订阅的 `dispose()` 操作，从而实现销毁变量的功能。

## 3. UIBindingObserver

