---
title: collections of protocol types
---

协议的集合类型：因为协议可以作为类型使用，可把遵从相同协议的实例放到一个协议类型的数组中。

一般情况下，数组是不能放不同类型的变量。

```swift
import UIKit

// 遵从同个协议的 4 个变量
let array : [CustomStringConvertible] = [1, 2, 3, "haha"]

for element in array {
    print(element)
}
```

灵活性大大增大，求同存异。