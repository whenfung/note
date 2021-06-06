---
title: error handling
---

 错误处理：反映运行出错的 “细节”，并恢复程序的过程。

- 可选类型的值缺失，错误处理可以针对不同的出错原因作出不同的处理。
- 一个函数可以加上 `throws` 关键字，表示可以处理错误，这个函数的调用者可以捕获（catch）这个错误并进行应付。
- 当调用可以抛出错误的函数，须在前面加上 `try` 关键字。
- 处理更细分的错误情况，错误类型须遵从 ErrorType 协议。
- 有时候仅关心结果有无，可使用 `try?` 或 `try!` 来忽略错误细节

```swift
import UIKit

// 抛出异常
func aFoo() throws {
    print("萨瓦迪卡")
}

try aFoo()

enum LearningObs:Error {
    case noMethod, noReading, noTool(tool:String)
}

func iosDev(method:Bool, style: Bool, hasTool:Bool) throws {
    guard method else {
        throw LearningObs.noMethod
    }
    
    guard style else {
        throw LearningObs.noReading
    }
    
    guard hasTool else {
        throw LearningObs.noTool(tool: "缺 Mac 电脑")
    }
}

var budget = 7000
func buy(tool:String) {
    if budget >= 6000 {
        budget -= 6000
        print("您已经购买", tool, "花费 6000 元，余额：",budget)
    } else {
        print("资金不足")
    }
}


// 捕获错误（推荐，鲁棒性）
do {
    try iosDev(method: true, style: true, hasTool: false)
    print("恭喜，您已经开始学习 iOS 开发！")
} catch LearningObs.noMethod {
    print("没有好的学习方法")
} catch LearningObs.noReading {
    print("不想看书，就看视频吧")
} catch LearningObs.noTool( _) {
    buy(tool: "Mac")
}

// 忽略错误细节
if (try? iosDev(method: true, style: true, hasTool: false)) != nil {
    print("恭喜进入学习！")
} else {
    // 具体什么条件我不关心
    print("学习条件不满足")
}

// 不满足条件没得谈，程序崩溃
try! iosDev(method: true, style: true, hasTool: false)
```

