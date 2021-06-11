---
title: Initializer Requirements
---

构造方法协议：可以要求遵从者实现指定的构造方法。

- 实现时用 `required init`，编译器会提示添加，无须手工添加 `required`。

```swift
import UIKit

protocol A {
    init(a:Int)
}

struct B:A {
    init(a: Int) {
    
    }
}

class C:A {
    // 类需要用 required 区分
    required init(a: Int) {
    }
}
```

