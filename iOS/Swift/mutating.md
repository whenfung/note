---
title: mutating
---

swift 中 `struct`、`enum` 均可以包含类方法和实例方法，`swift` 官方是不建议在 `struct`、`enum` 的普通方法里修改属性变量，但是在 `func` 前面添加 `mutating` 关键字之后就可以方法内修改。

对于 `protocol` 方法也是适用的，`mutating` 可以修饰的代理方法，如果 `struct`、`enum` 实现协议之后可以在对应的 `mutating` 代理方法内修改本身的属性变量。（`class` 不影响，因为属性变量对于类的类方法，实例方法是透明的，即随时都可以改变） 

- 结构体、枚举类型中声明修饰方法 

```swift
mutating func funcName()
```

- 结构体例子

```swift
extension String {
    mutating func appendString(aString: String) {
        self.append("test: \(aString)")
    }
}
```

- 协议例子

```swift
protocol MyProtocol {
    mutating func testMutatingKeyValue(index: Int)
}

struct MyStruct: MyProtocol {
    var index = 0
    mutating func testMutatingKeyValue(index: Int) {
        self.index = index
    }
}
```

- 在 swift 中，struct 和 enum 都存储在栈中，是值类型，传参是拷贝传参；class 是引用类型，传参是引用传参。