---
title: RxSwift subject
---

subject 是 observable 和 observer 之间的桥梁，一个 subject 既是一个 observable 也是一个 observer，他既可以发出事件，也可以监听事件。

对比一下 observable 和 observer 的区别，类似于 `observable.subcribe` 的语句，`observable` 就是被订阅的对象，创建的 observable 都是被别人订阅。而类似于 `observer.onNext` 语句，`observer` 就是发出事件的观察。

## 1. PublishSubject

PulishSubject ：订阅者只能接收订阅之后发出的事件（也就是说接收不到订阅之前代码做的修改）

从这里我们要学会如何制作事件源，如何订阅事件源，如何让事件源发出事件。

```swift
// pulishsubject，订阅者只能接收订阅之后发出的事件
let publishSubject = PublishSubject<String>()
publishSubject.onNext("18")
publishSubject.subscribe { (event: Event<String>) in
    print(event)
}.disposed(by: disposeBag)
publishSubject.onNext("coderwhy")
```

运行上面代码，我们可以发现只会输出订阅后的 `coderwhy`

## 2. ReplaySubject

ReplaySubject ：订阅者可以接收 bufferSize 个订阅前事件源发出的事件，如下：

```swift
// ReplaySubject，订阅者可以接受订阅之前或之后的事件
let replaySubject = ReplaySubject<String>.create(bufferSize: 2)
replaySubject.onNext("a")
replaySubject.onNext("b")
replaySubject.onNext("c")

replaySubject.subscribe { (event: Event<String>) in
    print(event)
}.disposed(by: disposeBag)

replaySubject.onNext("d")
```

如上，输出只有 b、c、d，如果你想接收所有订阅前事件源发出的事件，可将 `create(bufferSize: 2)` 改为 `createUnbounded()`。

## 3. BehaviorSubject

订阅者可以接收订阅之前的最后一个事件。

```swift
// BehaviorSubject，它会将源 `Observable` 中最新的元素发送出来
let behaviorSub = BehaviorSubject(value: "a")

behaviorSub.onNext("b")
behaviorSub.onNext("c")
behaviorSub.onNext("d")

behaviorSub.subscribe { (event: Event<String>) in
    print(event)
}.disposed(by: disposeBag)

behaviorSub.onNext("e")
behaviorSub.onNext("f")
behaviorSub.onNext("g")
```

输出是：d、e、f、g，这个类型在实际场景中还是很常用的，如果删除 b、c、d，那么就会接受初始值 a。

✅ 比如在网络中一开始加载网络中的很多数据，然后把数据初始化为 BehaviorSubject，默认加载到界面上，然后做下拉或上滑时，再接收信息作出响应。（太牛逼了）正在做到事件和反应随便分离。

⏰ `asObservable()` 可以将普通变量变为事件源。

## 4. PublishRelay

**PublishRelay** 就是 `PublishSubject` 去掉终止事件 `onError` 或 `onCompleted`。

## 5. BehaviorRelay

**BehaviorRelay** 就是 `BehaviorSubject`去掉终止事件 `onError` 或 `onCompleted`。





​	



