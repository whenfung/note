---
title: repeatElement
---

创建重复发出某个元素的 Observable。

```swift
// 创建重复发出 hello world 的序列
Observable.repeatElement("hello world")
		.subscribe(onNext: { element in
    		print(element)
    }).disposed(by: disposeBag)
```

如果想在第 5 次的时候终止序列，使用 `take` 操作符。

```swift
// 创建包含 5 个 hello world 的序列
Observable.repeatElement("hello world")
    .take(5)
    .subscribe(onNext: { element in
        print(element)
    }).disposed(by: disposeBag)
```

