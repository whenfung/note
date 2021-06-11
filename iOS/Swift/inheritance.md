---
title: inheritance
---

继承：class 之间的 “父子” 关系的体现

- 子类可以继承父类的属性和方法

```swift
import UIKit

class Vehicle {
    var speed = 0
    var desc : String {
        return "时速是\(speed)km/h"
    }
    func makeNoise() {
        
    }
}

class Bike : Vehicle {
    var hasBasket = true
}

let aBike = Bike()
aBike.speed = 30
aBike.desc
aBike.hasBasket
aBike.makeNoise()
```

- 子类可以根据需要，修改继承来的属性和方法（多态）

```swift
import UIKit

class Vehicle {
    var speed = 0
    var desc : String {
        return "时速是\(speed)km/h"
    }
    func makeNoise() {
        
    }
}

class CRH : Vehicle {
    override func makeNoise() {
        print("嘟嘟嘟")
    }
}

let crh = CRH()
crh.makeNoise()
```

