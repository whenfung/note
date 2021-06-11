---
title: function
---

## 匿名函数

```swift
{
    //代码体
}()
```

大括号 `{}` 是定义函数体的，小括号 `()` 是调用函数体的。

为什么不写一个函数然后调用函数呢？为何用匿名函数呢？

主要还是代码的简洁性.

举个例子：

```swift
 private lazy var guideScrollView: UIScrollView = {
        let view = UIScrollView.init()
        view.backgroundColor = UIColor.lightGray
        view.bounces = false
        view.isPagingEnabled = true
        view.showsHorizontalScrollIndicator = false
        view.delegate = self
        return view
    }()
```

上述代码简单清晰明了，如果使用函数，要先写一个函数，然后再去调用，麻烦许多，代码也不明朗。

当时，你也可以使用闭包来这样初始化

```swift
 private lazy var guideScrollView = { () -> UIScrollView in
        let view = UIScrollView.init()
        view.backgroundColor = UIColor.lightGray
        view.bounces = false
        view.isPagingEnabled = true
        view.showsHorizontalScrollIndicator = false
        view.delegate = self
        return view
    }
```

## 函数类型

函数可作为参数，例如返回类型为 `Void`，可以直接写成 `()`，如下

```swift
import UIKit

func myPrint() -> Void {
    print("你好")
}

func myHello(name: String) -> Void {
    print("Hello, \(name)")
}

// () 是 Void 的意思，也就是说所有返回类型为空的函数
var array: [()] = Array<()>()

array.append(myPrint())
array.append(myHello(name: "你好"))

print(array.count)
```

同时，函数类型可以作为函数返回值。

```swift
import UIKit

func max(a: Int, b: Int) -> Int
{
    return a > b ? a : b
}

func min(a: Int, b: Int) -> Int
{
    return a < b ? a : b
}

func chooseFunc(getMax: Bool) -> (Int , Int) -> Int
{
    return getMax ? max : min
}

var myFunc:(Int , Int) -> Int = chooseFunc(getMax: false)

print(myFunc(10, 20))
```

