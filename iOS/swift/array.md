---
title: array
---

数组是同类型的值的有序组合。

```swift
import UIKit

// 定义
let array : [Int]

// 创建 10 个 3 的整型数组
array = [Int](repeating: 3, count: 10)

// 创建一个有序范围的 Int 数组
let array2 = Array(1...10)

// 用数组字面量来创建数组（最常用）
var places = ["beijing", "shanghai", "guangzhou", "shenzhen"]

// 元素计数 count
places.count

// 元素空否
places.isEmpty

// 添加
places.append("wuhan")

// 数组合并
let haiwaiPlace = ["NewYork", "London", "Sao paolu"]
places += haiwaiPlace

// 获取，下标访问，下标不能超
// index out of range
places[3]

// 插入
places.insert("Paris", at: 5)

// 移除
places.remove(at: 5)
```

## 注意

- 如果要创建元素是对象的数组，不能使用 `[](repeating, count)` 进行初始化，因为这样导致数组中的所有元素都指向同一个对象，实际上不论你 `repeating` 多少次，数组元素都相当于一个指针，指向同一个对象。

  如果想创建数组元素是不同对象的数组，只能通过逐个分配的方式，例如创建 `UIButton` 的数组，一般使用 `for-in` 配合数组的 `append` 方法逐个添加。

