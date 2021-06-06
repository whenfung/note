---
title: dictionary
---

字典是键值对组成的数组。

```swift
import UIKit

// 定义
var a: Dictionary<String, String>
var b: [String:String]

// 用字典字面量来创建字典，键与值之间用冒号隔开
var airports = ["PVG" : "Shanghai Pudong", "CHU" : "Dalian", "DUB" : "Dublin Airport"]

// 计数: count
airports.count

// 空否
airports.isEmpty

// 添加
airports["SHQ"] = "Hongqiao Airport"
airports["CHU"] = "大连周水子机场"

// 获取
airports["SHQ"]

// 移除，把下标设为 nil
airports["DUB"] = nil

// 循环一个字典 for in，因为键值对有 2 个元素，用元组变量
for (key, value) in airports {
    print(key, value)
}

// 单独使用其中的键或值，使用 keys 或 values（可使用 for in）
for key in airports.values {
    print(key)
}

// 把键值对分离成数组
let codes = [String](airports.keys)
let airportFullname = [String](airports.values)
```

