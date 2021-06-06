---
title: Single
---

只发出可观察到的第一项（或满足某些条件的某一项）。如果只是使用 `single` 的话，如果可观察者的元素多于 1 个会发送 `error` 事件，并停止发送后面的事件。

- 多个事件的序列

```swift
Observable<Int>.of(1, 2, 3)
    .single()
    .subscribe(onNext: { element in
        print("element:", element)
    }, onError: { error in
        print("error:", error)
    })
    .disposed(by: disposeBag)
```

输出是：

```bash
element: 1
error: Sequence contains more than one element.
```

- 符合条件的序列

```swift
Observable<Int>.of(1, 2, 3)
    .single { $0 == 3 }
    .subscribe(onNext: { element in
        print("element:", element)
    })
    .disposed(by: disposeBag)
```

输出是：

```bash
element: 3
```

