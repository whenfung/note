---
title: Generics
---

类似 `C++` 的模，swift 提供泛型。例如交换两个相同类型的参数，可以实现：

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}


var someInt = 3
var anotherInt = 107
swapTwoValues(&someInt, &anotherInt)
print("someInt: \(someInt)")
print("anotherInt: \(anotherInt)")
```

在这里 `inout` 参数表示如果输入输出参数，其实就是 `C++` 里头的引用，当加上这个参数后，调用函数的时候，传入的参数必须加上 `&` 符。

