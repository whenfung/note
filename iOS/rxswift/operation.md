---
title: operation
---

之前我们介绍的都是一些系统控件固定的操作，除了官方库给的这些控件上的 observable 操作，我们还可以自己创建。

下面介绍常见的 observable 操作。

tips：动态库需要在 embeded binaries 中加入。

如果你不是用控件（UIKit 里的东西），那么你就不需要 `import RxCocoa`。

## 1. never

tips ：subscribe 的 on 方法表示不管是 next、error 还是 completed，都会执行到 on ；而如果使用 onNext 或其他，则会细化到某个动作。

Never 操作符将创建一个 Observable，但这个 observable 不会产生任何事件。（内心：我靠，生成一个事件源，但这个事件源不会产生任何事件，有啥用？）

## 2. empty

创建一个空 observable

**empty** 操作符将创建一个 `Observable`，这个 `Observable` 只有一个完成事件。（事件源为空，那么肯定发个完成事件啦）

## 3. of

```swift
// of
let ofO = Observable.of("a", "b")
ofO.subscribe { (event: Event<String>) in
    print(event)
}.disposed(by: disposeBag)
```

## 4. from

```swift
// from
let array = [1, 2, 3, 4, 5]
let fromO = Observable.from(array)
fromO.subscribe { (event: Event<Int>) in
    print(event)
}.disposed(by: disposeBag)
```

## 5. create

通常情况下一个有限的序列，只会调用一次**观察者**的 `onCompleted` 或者 `onError` 方法。并且在调用它们后，不会再去调用**观察者**的其他方法。（也就是说调用了 onCompleted 之后就不会掉用后面的 onNext 事件）

```swift
// create
let createO = createObservable()
createO.subscribe { (event: Event<Any>) in
    print(event)
}.disposed(by: disposeBag)

private func createObservable() -> Observable<Any> {
    return Observable.create { (observer: AnyObserver<Any>) -> Disposable in
        observer.onNext("coderwhy")
        observer.onNext("18")
        observer.onNext("1.88")
        observer.onCompleted()
        
        return Disposables.create()
    }
}
```

## 6. range

```swift
// range
let rangeO = Observable.range(start: 1, count: 10)
rangeO.subscribe { (event: Event<Int>) in
    print(event)
}.disposed(by: disposeBag)
```



