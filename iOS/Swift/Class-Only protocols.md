---
title: Class-Only Protocols
---

类专用协议：可以把协议限制在 `class` 类型（让结构体和枚举无法使用），加关键字 `AnyObject` 到协议继承列表的第一位。

```swift
import UIKit

// 类专用协议
protocol OnlyForClass : AnyObject, CustomStringConvertible {
    
}

class MyText : OnlyForClass {
    var description: String {
        return "oh my god"
    }
}
```

## 参考

- [Class-Only Protocols](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html#ID281)

