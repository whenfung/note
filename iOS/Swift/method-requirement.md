---
title: method requirement
---

方法协议：定义时没有花括号执行体，实现仅要求名称相同。

- 作用：可以让一个类/结构体/枚举的方法，分解为更小的组合，从而更灵活。

类型方法协议：前缀总是 `static`。

```swift
import UIKit

// 协议里的类型方法只能用 static，而不能使用 class
protocol AMethod {
     static func foo()
}

class A : AMethod {
    // 可以实现为 static，若可覆盖，可实现为 class
    static func foo() {
        print("aaaa")
    }
}
```

实例方法协议

```swift
import UIKit
import Foundation

// 实例方法协议，每个结构可以实现不一样的随机数方法
protocol RandomGeneratable {
    func randomNumber() -> Int
}

struct RandomNumber : RandomGeneratable {
    func randomNumber() -> Int {
        // Foundaton 的一个方法
        return Int(arc4random())
    }
}

struct RandomNumberInSix : RandomGeneratable {
    func randomNumber() -> Int {
        return Int(arc4random()) % 6
    }
}

let random1 = RandomNumber()
random1.randomNumber()

let random2 = RandomNumber()
random2.randomNumber()
```

结构体/枚举的 “变异方法协议” ：和类不同，结构体或枚举要使用自己的方法修改自己的属性，必须添加 `mutating` 关键字

```swift
import UIKit

protocol Switchable {
    mutating func onOff()
}

enum MySwitch: Switchable {
    mutating func onOff() {
        switch self {
        case .off:
            self = .on
        default:
            self = .off
        }
    }
    
    case on, off
}
```

