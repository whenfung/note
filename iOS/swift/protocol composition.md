---
title: protocol composition
---

我们可以临时将多个协议组合起来，协议的名字一定要形容词

```swift
import UIKit

protocol Ageable {
    var age: Int { get }
}

protocol Nameable {
    var name: String { get }
}

// 协议组合
func wish(someone: Ageable & Nameable) {
    print("祝", someone.name, someone.age, "岁生日快乐！")
}

struct Student: Ageable, Nameable {
    var age: Int
    var name: String
}

let student = Student(age: 10, name: "Tom")
wish(someone: student)
```

