---
title: enum
---

- 枚举是有限种情况都列举，例如天气、颜色、星期等

```swift
import UIKit

// 定义
enum Weather {
    case sunny
    case cloudy
    case rainy
    case snow
    case froggy
}

// 使用
Weather.cloudy

// 与 switch 配合使用。如果变量是枚举值，可省略枚举名
var todayweather = Weather.sunny

switch todayweather {
case .cloudy:
    print("今天天气多云")
case .sunny:
    print("天气晴朗")
default:
    print("天气情况未知")
}

// tip : 颜色可认为枚举变量

// 每一种类型都可附加一个或多个值，形式是元组
enum 精确天气 {
    case 晴(Int, Int, String)
    case 霾(String, Int)
}

// 赋予附加值
let 上海今日精确天气 = 精确天气.霾("PM10", 100)
let 北京今日精确天气 = 精确天气.晴(30, 50, "湛蓝")

switch 北京今日精确天气 {
case .晴(let uvi, let li, let desc):
    print("紫外线指数:",uvi,"晾晒指数:",li,"天蓝指数:",desc)
case .霾(let cat, let index):
    print("雾霾颗粒类别:", cat, "指数:", index)
}
```

- 除了枚举，在 swift 中 enum 还可以作为命名空间使用，如下：

```swift
enum Colors {
    enum Text {
        static let light = 0x36b59d.toUIColor()
    }
    
    enum View {
        static let light = 0x36b59d.toUIColor()
    }
}
```

然后当你要使用颜色的时候，方式如下：

```swift
view.backgroundColor = Colors.View.light
```

注意，变量都需要加上 `static`，因为 `toUIColor` 是个函数，所以决定了 alert 是存储属性，enum 中不能包含存储属性。