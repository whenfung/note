---
title: 属性协议
---

属性协议：要求遵从者以指定的名称实现属性，但具体实现是实例属性或类型属性并不关心。

- 可以制定要求实现 `getter` 或 `getter + setter`。属性必须定义为 `var`。

属性分为实例属性和类型属性

```swift
// 实例属性协议用得较多
protocol FileAccessPriority {
    var readOnly : Bool { get }
    var readWrite: Bool { get set }
}

// 类型属性协议
protocol AccessUrl {
    static var link:String { get }
}
```

特征可提取为协议

```swift
import UIKit

// 名字的特性提取
protocol FullName {
    var fName:String { get }
    var gName:String { get }
}

struct Student: FullName {
    var fName: String
    var gName: String
    
}

struct Teacher: FullName {
    var fName: String
    var gName: String
}

var student1 = Student(fName: "王", gName: "小明")
student1.fName
student1.gName
```

可实现为计算属性

```swift
import UIKit

// 名字的特性提取
protocol FullName {
    var fName:String { get }
    var gName:String { get }
}

// 大人物的 FullName 可能不一样，可以使用计算属性
class SomeBody : FullName {
    var title: String?
    var name: String
    
    init(title:String?, name:String) {
        self.title = title
        self.name = name
    }
    
    var fName: String {
        return title ?? "无名下将"
    }
    
    var gName: String {
        return name
    }
    
    var desc:String {
        return self.fName + self.gName
    }
}

var somebody1 = SomeBody(title: "大帝", name: "亚历山大")
somebody1.gName
somebody1.fName
somebody1.desc

var nobody = SomeBody(title: nil, name: "小邓")
nobody.fName
nobody.gName
nobody.desc
```

