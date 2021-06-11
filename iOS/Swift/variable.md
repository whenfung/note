---
title: variables
---

变量统一用 `var`，大部分时候可类型推断

```swift
import UIKit

// 定义后类型不可改变
var 跑步多少公里 : Int = 5

// Int 类型推断
var 明天公里数 = 6
```

常量 `let` 不能和 `lazy` 一起使用。