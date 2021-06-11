---
title: optional
---

`optionals` 是 `swift` 的一个重要特性，这是 `objc` 所没有的概念。

```swift
import UIKit

class BlogPost {
    // 这里的 ? 表示 title 可以为空，可以不需要初始化，因此也就不需要初始方法
    var title:String?
}

let post = BlogPost()

// Optional Binding
if let actualTitle = post.title {
    print(actualTitle)
} else {
    print("字符串为空")
}

// 如果你直接使用下面句子，则会有警告，你必须考虑字符串为空的情况
print(post.title)

// 例如提供默认值
print(post.title ?? "hello")

// 如果你确定你的 title 已经有值了，你可以强制解包
// 当然如果强制对 nil 解包，则会报错
// Unexpectedly found nil while unwrapping an Optional value
print(post.title!)

// 当然你可以用传统风格来处理，如下
if post.title != nil {
    print(post.title!)
}
```

应用场景：用户资料的选填

```swift
import UIKit

// 地址选填
var addr: String?
addr = "上海海事大学"

```

## 补充

1. `as!` 是强制转换，若失败则程序崩溃；`as?` 是可选转换，即使失败程序也不会崩溃。
2. 如果是可选类型，没有加 `?` 或 `!` 解析操作，那么输出的是 `optional(value)`