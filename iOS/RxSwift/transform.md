---
title: transform
---

变换操作就是将一个序列变为另一个序列。

## 1. map

如果用 swift 的方式求一个数组的平方，那么有下面几种方式：

```swift
// for
let array = [1, 2, 3, 4]
var array1 = [Int]()
for num in array {
	  tempArray.append(num * num)
}

// map
// 返回类型是一个泛型，这里我们设置为 Int
let array2 = array.map { (num: Int) -> Int in
    return num * num
}

// 简化的 map
let array3 = array.map({ $0 * $0})
print(array3)
```

那么在 RxSwift 中的 map 是：

```swift
// RxSwift map
Observable.of(1, 2, 3, 4)
    .map { (num: Int) -> Int in
        return num * num
}.subscribe { (event: Event<Int>) in
    print(event)
}.disposed(by: disposeBag)
```

输出 5 个事件：1、4、9、16、completed。

## 2. flatMap

如果结构体里面的变量是事件源，那么如果你做订阅的时候就会变成套娃模式，而 flatMap 可以让你的代码风格变成链式。

假设有那么一个结构体如下：

```swift
struct Player {
    init(score: Int) {
        self.score = BehaviorSubject(value: score)
    }

    let score: BehaviorSubject<Int>
}
```

我们要订阅成员变量 score，我们可以直接使用 `.` 运算符，然后订阅 score，但是也可以直接获取 Player 序列，然后转为 score 序列，然后订阅，如下：

```swift
let 👦🏻 = Player(score: 80)
let 👧🏼 = Player(score: 90)

// 这里的 value 就是一个事件源
let player = BehaviorSubject(value: 👦🏻)

// Player 中的 score 也可作为一个事件源
player.asObservable()
    .flatMap { $0.score.asObservable() } // Change flatMap to flatMapLatest and observe change in printed output
    .subscribe(onNext: { print($0) })
    .disposed(by: disposeBag)

👦🏻.score.onNext(85)

// 产生一个新的事件源
player.onNext(👧🏼)

👦🏻.score.onNext(95) // Will be printed when using flatMap, but will not be printed when using flatMapLatest

👧🏼.score.onNext(100)
```

此时，依次输出 80、85、90、95、100。

flatMapLatest ：将 `Observable` 的元素转换成其他的 `Observable`，然后取这些 `Observables` 中最新的一个。

如果将上述的 `flatMap` 改为 `flatMapLatest`，那么 95 就不会发出了，因为 `flatMapLatest` 会解除所有旧事件源的订阅。

