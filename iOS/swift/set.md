---
title: set
---

值无序不重复，适合存储具有唯一性的数据

```swift
import UIKit

// 定义，注意 1 会被合并
let cardno : Set = [1, 2, 3, 1]

// 用数组字面量创建集合
var citys : Set = ["Shanghai", "Beijing", "wuhan", "Hefei"]

// 计数 count
citys.count

// 空否 isEmpty，集合必须给值
cardno.isEmpty

// 插入
citys.insert("guangzhou")

// 移除
citys.remove("Shanghai")

// 是否包含某元素
citys.contains("Beijing")

// 转化为数组
let citysArray = citys.sorted()


// --- 集合运算
let x:Set = [1,2,3,4]
let y:Set = [3,4,5,6]

// 交集
x.intersection(y)

// 补集
x.subtracting(y)

// 并集
x.union(y)

// 差集
x.symmetricDifference(y)

// --- 集合之间的关系
let h:Set = [1,2,3]
let i:Set = [3,2,1]

// 相等
h == i

// 子集
h.isSubset(of: i)

// 严格子集
h.isStrictSubset(of: i)

// 父集
h.isSuperset(of: i)

// 严格父集
h.isStrictSuperset(of: i)

let j:Set = ["游戏", "动漫"]
let k:Set = ["购物", "小吃", "化妆"]

// 无交集
j.isDisjoint(with: k)
```

