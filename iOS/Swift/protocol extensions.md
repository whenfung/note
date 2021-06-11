---
title: 协议拓展
---

协议拓展：无须知道源码的情况下，给已有的类添加协议。

- 即存实例会自动遵从添加了的协议

```swift
import UIKit

let a = 1

protocol ShowHint {
    func hint() -> String
}

extension Int: ShowHint {
    func hint() -> String {
        return "整数：\(self)"
    }
}

a.hint()
(-4542).hint()
```

- 如果一个类型预先遵从了协议，可以直接拓展协议

```swift
import UIKit

struct Lesson {
    var name:String = "deng"
    
    // 系统已经有该协议
    var description:String {
        return "课程名是：" + name
    }
}

// 预遵从
extension Lesson: CustomStringConvertible{}
var lesson = Lesson()
lesson.description
```

💡拓展协议：可以在拓展协议的同时，加上限定条件（`where` 语句）

```swift
import UIKit

protocol ShowHint {
    func hint() -> String
}

extension Int: ShowHint {
    func hint() -> String {
        return "整数：\(self)"
    }
}

// 协议 ShowHint 被拓展，只要遵从了 CustomStringConvertible 就会可拓展
// 限制在 CustomStringConvertible 协议中
extension ShowHint where Self: CustomStringConvertible {
    func hint1() -> String {
        return "我是一个能显示成字符串的类型" + self.description
    }
}

1.hint1()
```

集合类型 `Collection` 也是一种协议，`Iterator.Element` 指代其中的元素

```swift
import UIKit

let array = [1, 2, 3, 4]

extension Collection where Iterator.Element : CustomStringConvertible {
    func newDesc() -> String {
        // 将所有元素的描述映射数组存到 itemAsText
        let itemAsText = self.map { $0.description }
        // 以 "," 分割 itemAsText 数组并拼接成 string
        return "该集合类型元素数目是\(self.count), 元素的值依次是" + itemAsText.joined(separator: ",")
    }
}

array.newDesc()
print(array)
```

