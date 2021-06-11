---
title: closures
---

闭包是没有名字的函数。

```swift
import UIKit

var city = ["zhengzhou", "xiamen", "hefei", "nanchang"]

// 1. sorted 函数排序
var cityRank1 = city.sorted()

// 2. 使用函数排序，不简洁
func daoxu(a: String, b:String) -> Bool {
    return a > b
}

var cityRank2 = city.sorted(by: daoxu)

// 3. 使用闭包排序
var cityRank3 = city.sorted { (a, b) -> Bool in
    return a > b
}

// 4. 闭包的自动推断
// 参数和返回类型可自动推断，单表达式可以忽略 return 关键词
var cityRank4 = city.sorted { (a, b) in
    a > b
}

// 5. 使用快捷参数，前缀 $，从 0 开始递增
var cityRank5 = city.sorted{ $0 > $1 }
```

## map

map 方法接受一个闭包作为参数， 然后它会遍历整个集合，并对集合中每一个元素执行闭包中定义的操作。 相当于对集合中的所有元素做了一个映射。例如下面的代码中，map 方法会接收一个闭包作为参数，然后对数组 numbers 的所有元素做一个映射。

```swift
let numbers = [1, 2, 3, 4]
let result = numbers.map { $0 + 2 }
print(result)
```

## flatMap

map 可以对一个集合类型的所有元素做一个映射操作。 那么 flatMap 呢？如果你将上述的 map 操作改为 flatMap 操作，可以发现得到的结果是一样的，如下：

```swift
let numbers = [1, 2, 3, 4]
let result = numbers.flatMap { $0 + 2 }
print(result)
```

那么 flatMap 有什么不同？

```swift
let numbersCompound = [[1,2,3],[4,5,6]];
var res = numbersCompound.map { $0.map{ $0 + 2 } }
// [[3, 4, 5], [6, 7, 8]]
var flatRes = numbersCompound.flatMap { $0.map{ $0 + 2 } }
// [3, 4, 5, 6, 7, 8]
```

对于二维数组， map 和 flatMap 的结果就不同了。 map 和 flatMap 都会对每个元素做映射操作，但是 flatMap 还会执行 “降维操作”。





