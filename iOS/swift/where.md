---
title: where
---

类似于数据库，`where` 是个限定词。

- do catch

```swift
import UIKit

enum ExceptionError: Error {
    case httpCode(Int)
}

func throwError() throws {
    throw ExceptionError.httpCode(500)
}

// do catch
do {
    try throwError()
} catch ExceptionError.httpCode(let httpCode) where httpCode >= 500 {
    print("server error")
}
```

- switch

```swift
import UIKit

var value: (Int, String) = (1, "小明")
switch value {
case let (x, y) where x < 60:
    print("\(y)不及格")
default: break
}
```

- for in

```swift
import UIKit

let array = [1, 2, 3, 4, 5]
let dictionary = [1: "siri", 2: "小爱同学"]

for i in array where dictionary[i] != nil {
    print(i)
}
```

- 协议（protocol）

```swift
import UIKit

protocol aProtocol {}

extension aProtocol where Self:UIView {
    // 只给遵守 aProtocal 协议的 UIView 添加拓展
    // 面向协议编程
    func getString() -> String {
        return "string"
    }
}

class MyView: UIView {}
extension MyView: aProtocol {}
let myView = MyView()
let aStr = myView.getString()
```

- `Swift3.0` 之前在 `if let` 语句中可使用 where 语句，现在改为：

```swift
import UIKit

var swift4: String? = "swift4 来了"
if let swift4Srt = swift4, swift4Srt.hasPrefix("swift4") {
    print(swift4Srt)
}
```

- `Swift3.0` 之前在 `guard ... let` 语句中可使用 where 语句，现在改为：

```swift
import UIKit

var swift4: String? = "swift4 来了"

guard let swift4Srt = swift4, swift4Srt.hasPrefix("swift4") else {
    print("不在")
    fatalError()
}
```

- `Swift3.0` 之前在 `while` 语句中可使用 where 语句，现在改为：

```swift
import UIKit

var arrayTwo:[Int]? = []
while let arr = arrayTwo , arr.count < 5 {
    arrayTwo?.append(1)
}
```

