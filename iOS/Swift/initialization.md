---
title: initialization
---

实例的属性需要初始化。

```swift
import UIKit

class RoomTemp {
    // 使用默认值初始化
    var name = "MeiZhou"
    
    // 类的定义没有给属性默认的值，则需要值 init 中指定
    var season : String
    var temp: Int
    
    init(season: String, temp: Int) {
        // 加 self 的目的是区分参数和属性
        self.season = season
        self.temp = temp
    }
}

let temp1 = RoomTemp(season: "Spring", temp: 20)
temp1.season
temp1.temp
```

结构体定义不需要指定属性默认的值，因为默认提供一个包含所有属性初始化的构造器。

```swift
import UIKit

struct RoomTemp {
    var season : String
    var temp : Int
}

let temp2 = RoomTemp(season: "Summer", temp: 30)
```

便利构造器：可以通过对主构造器的包装，实现便利的初始化

```swift
import UIKit

class Food {
    var name: String
    
    init(name: String) {
        self.name = name
    }
    convenience init() {
        self.init(name:  "饭食")
    }
}

let food = Food()
```

可失败构造器：针对有可能的初始化失败，返回 `nil`，例如初始化一张图片，如果图片名不存在，则初始化失败。

```swift
import UIKit

class Animal {
    let name:String
    
    init?(name:String) {
        if name.isEmpty {
            print("没有给动物命名！")
            return nil
        }
        self.name = name
    }
}

let cat = Animal(name: "")
```

再举一个图片的例子。

```swift
import UIKit

// 若 ball 不存在，则返回 nil
let ball = UIImage(named: "ball")
```

