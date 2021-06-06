---
title: protocols as types
---

协议作为类型使用：可用于参数类型/返回类型、变量/常量/属性、集合类型中的元素类型。

```swift
import UIKit

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

struct Dice {
    var sides :Int
    // 符合随机数协议，协议可作为类型，理解为满足 RandomGeneratable 协议的变量
    var randomNumber : RandomGeneratable
    
    func play() -> Int {
        return self.randomNumber.randomNumber() % sides + 1
    }
}

let aDice = Dice(sides: 6, randomNumber: RandomNumber())
aDice.play()
```

当然你可以将协议实现为一个类，然后再作为类型使用

```swift
import UIKit

// 实例方法协议，每个结构可以实现不一样的随机数方法
protocol RandomGeneratable {
    func randomNumber() -> Int
}

class TenRandomNumber : RandomGeneratable {
    func randomNumber() -> Int {
        return Int(arc4random()) % 10 + 1
    }
}

struct Dice {
    var sides :Int
    // 符合随机数协议，协议可作为类型，理解为满足 RandomGeneratable 协议的变量
    var randomNumber : RandomGeneratable
    
    func play() -> Int {
        return self.randomNumber.randomNumber() % sides + 1
    }
}

let aDice = Dice(sides: 6, randomNumber: TenRandomNumber())
aDice.play()
```

