---
title: RxRelay
---

**RxRelay** 既是 **可监听序列** 也是 **观察者**。

他和 **Subjects** 相似，唯一的区别是不会接受 `onError` 或 `onCompleted` 这样的终止事件。

## BehaviorRelay

```swift
let disposeBag = DisposeBag()

let getInt = BehaviorRelay<Int>(value: 1)
getInt.accept(1)

getInt.subscribe(onNext: { element in
    print("element: \(element)")
    }).disposed(by: disposeBag)
getInt.accept(2)
```

输出如下：

```swift
element: 1
element: 2
```

可以看出订阅者只能收到订阅前的最后一个元素（两个 1 只收到了一个 1）。此后的事件都能收到。但是不会处理 error 事件和 complete 事件。