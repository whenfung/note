---
title: Protocol Inheritance
---

💡 协议继承：一个协议可以继承若干个协议，并可以在继承基础上增加新需求，与 `class` 继承相似，区别是 `class` 不能多重继承。

对结构体进行协议拓展，相当于实现了多重继承（面向协议编程）。

- 继承的多个协议间用逗号分割

💡 提供默认实现：可以给协议拓展提供一个默认的实现，任何遵从此协议的类型都会获得。

```swift
import UIKit

protocol Workable {
    func work()
}

extension Workable {
    func work() {
        print("I'm carrying bricks")
    }
}

// 由于有了 work() 的默认实现，因此 Robot 可以不实现 work()
// 如果 Robot 重写了 work()，就会覆盖默认的实现
class Robot:Workable {
    
}

let robot = Robot()
robot.work()
```

鼓励把类的继承改成协议的模块继承。