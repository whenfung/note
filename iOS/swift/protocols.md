---
title: protocols
---

协议：方法、属性或一段功能的蓝本（不实现）

- 协议可被类、结构体和枚举 “领养” 从而 “长大成人”
- 任何满足协议的 “需求” 的类型，被称为 “遵从” 该协议
- “领养/遵从” 若干个协议，用逗号分割
- 父类在协议之前

可类比继承，但是继承只用于类。

```swift
import UIKit

// 定义
protocol AProtocol {
    
}

protocol BProtocol {
    
}

// 遵从协议，逗号隔开
struct AStruct : AProtocol, BProtocol {
    
}

// 类比 class
class Name {
    
}

class GiveName : Name {
    
}
```

