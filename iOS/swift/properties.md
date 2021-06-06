---
title: properties
---

属性是一个类、结构体、枚举关联的变量。swift 的属性有三种：

- 存储属性：保存单个类型的变量。
- 计算属性：由其他属性计算得出的，由 getter 和 setter 组成。
- 类型属性：与实例无关的属性。

其中，存储属性和计算属性都属于实例属性。

## 1. 存储属性

```swift
import UIKit

class 角色 {
    
    // 存储属性
    var id = ""
    var money = 0
}

let deng = 角色()

deng.id = "ProfessorDeng"
deng.money = 5000

```

## 2. 计算属性

```swift
import UIKit

class 角色 {
    
    // 存储属性
    var id = ""
    var money = 0
}

let deng = 角色()

deng.id = "ProfessorDeng"
deng.money = 5000


struct 坐标 {
    var x = 0, y = 0
}
enum 移动方式 {
    case 走
    case 跑
    case 骑
    case 传送
}

class 法师: 角色 {
    var 人物坐标 = 坐标()
    var 人物移动方式 = 移动方式.走
    
    // 计算属性
    var 当前坐标: 坐标 {
        get {
            switch 人物移动方式 {
            case .走:
                人物坐标.x += 1
                人物坐标.y += 1
            case .跑:
                人物坐标.x += 3
                人物坐标.y += 3
            case .骑:
                人物坐标.x += 10
                人物坐标.y += 10
            case .传送:
                人物坐标.x += 1000
                人物坐标.y += 1000
            }
            return 人物坐标
        }
        // newValue 是默认值，set 可以没有
        set {
            人物坐标 = newValue
        }
    }
}

var 法师1 = 法师()
法师1.当前坐标

法师1.人物移动方式 = .跑
法师1.当前坐标

法师1.人物移动方式 = .传送
法师1.当前坐标

// 计算属性的 setter 方法，影响其他属性
法师1.当前坐标 = 坐标(x: 2000, y: 90)
法师1.人物坐标
```

## 3. 属性监视器

对属性值的变化进行响应。

- `willSet` : 事前响应。新值 newValue
- `didSet` : 事后响应。旧值 oldValue

```swift
import UIKit

class 经验 {
    var 总经验 = 0 {
        willSet {
            print("当前总经验是:\(newValue)!")
        }
        didSet {
            print("增加了\(总经验 - oldValue) 点经验!")
        }
    }
}

var 经验1 = 经验()
经验1.总经验 = 1000
经验1.总经验 = 3000
经验1.总经验 = 8000
```



