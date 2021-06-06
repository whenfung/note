---
title: method
---

类里的方法可分为实例方法和类型方法。

方法 & 函数 ：面向过程的功能模块称为函数，面向对象中类里面的功能称为方法。

函数：是一段完成特定任务的独立代码片段。你可以通过给函数命名来标识某个函数的功能。

方法是与某些特定类型相关联的函数。

## 1. 实例方法

实例化之后才可以使用实例方法。

💡 `self` 属性：引用实例自身，通常可以不写。 

## 2. 类型方法

不需要实例化，类点法即可调用。

```swift
import UIKit

class Player {
    static var nick = "deng"
    
    class func server() {
        print("\(nick), 您在北京联通 1 区")
    }
}

Player.server()

class ShanghaiPlayer : Player {
    // 如果没有子类了，那么 class 可以改为 static
    override class func server() {
        print(nick, "您在上海电信 2 区")
    }
}

ShanghaiPlayer.server()
```

