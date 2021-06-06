---
title: 协议的检查与转换
---

协议的检查和转换：使用 `is` 和 `as` 类型转换操作符来检查协议遵从与否，或转换成特定的协议。

 ```swift
import UIKit

protocol Slogan {
    var desc: String { get }
}

protocol Coder: Slogan {
    var name: String { get set }
}

struct JavaCoder : Coder {
    var name:String
    var desc: String {
        return "我会 Java"
    }
}

struct JSCoder: Coder {
    var name: String
    var desc: String {
        return "我会 JS"
    }
}

struct NewBie {
    var name:String
}

let java1 = JavaCoder(name: "小木")
let js1 = JSCoder(name: "小兰")
let newbie1 = NewBie(name: "小丰")

// 任意类型
let coders = [java1, js1, newbie1] as [Any]

for coder in coders {
    if let coder = coder as? Coder {
        print(coder.name, coder.desc)
    } else {
        print("你不是一个程序员！")
    }
    if let newbie = coder as? NewBie {
        print(newbie.name, "你是菜鸟")
    }
}
 ```

