---
title: share
---

`shareReplay` 会返回一个新的事件序列，它监听底层序列的事件，并且通知自己的订阅者们。

`shareReplay` 解决了有多个订阅者的情况下，`map` 会被执行多次的问题。

```swift
let subject = PublishSubject<Int>()

let observable = subject
    .map { i -> Int in
        print("map --- \(i)")
        return i
    }
    .share(replay: 1, scope: .forever)
        
observable.subscribe(onNext: { num in
    print("--- 1 --- \(num)")
    }).disposed(by: disposeBag)

subject.onNext(1)
subject.onNext(2)

observable.subscribe(onNext: { num in
    print("--- 2 --- \(num)")
    }).disposed(by: disposeBag)

subject.onNext(3)
subject.onNext(4)

observable.subscribe(onNext: { num in
    print("--- 3 --- \(num)")
    }).disposed(by: disposeBag)

subject.onCompleted()
```

上述代码输出：

```bash
map --- 1
--- 1 --- 1
map --- 2
--- 1 --- 2
--- 2 --- 2
map --- 3
--- 1 --- 3
--- 2 --- 3
map --- 4
--- 1 --- 4
--- 2 --- 4
--- 3 --- 4
```

可以看出，不管有多少个观察者，只要使用了 `.share` 语句，每个 `onNext` 只会 `map` 一次。而且我们注意到最后的 `subscribe` 可以接收到订阅之前的 `onNext(4)`，这是 `replay` 的作用，你可以尝试调整 `replay` 的个数。

如果去掉 `.share(replay: 1, scope: .forever)` 语句，输入如下：

```bash
map --- 1
--- 1 --- 1
map --- 2
--- 1 --- 2
map --- 3
--- 1 --- 3
map --- 3
--- 2 --- 3
map --- 4
--- 1 --- 4
map --- 4
--- 2 --- 4
```

可以发现每一个观察者都会调用一次 `map`。而且新订阅者得不到上次发送的信息。