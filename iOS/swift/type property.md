---
title: type properties
---

类型属性：属于类型固有的，实例不能调用。

⚠️ 存储属性的前面加关键字 `static`。

⚠️ 计算属性的前面加关键字 `class` 可被子类重写（`override`）。

```swift
import UIKit

class 生命体 {
    // 类变量
    class var 遗传方式 : String {
        return "RNA"
    }
}

生命体.遗传方式

class Human: 生命体 {
    override class var 遗传方式: String {
        return "DNA"
    }
}

Human.遗传方式
```

在结构体和枚举中使用 `static`

```swift
struct 逛街 {
    static let 试衣间 = "UNICIO"
    static let 网站 = "http://www.taobao.com?cate="
    var cate = ""
    
    var shareURL : String {
        return 逛街.网站 + cate
    }
}

逛街.试衣间

let 逛淘宝 = 逛街(cate: "seafood")

逛淘宝.shareURL
```



