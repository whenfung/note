---
title: CodingKeys
---

`API` 更改键的名称，如 `id` 改为 `employeeID`，解决方法：

```swift
struct Employee: Codable {
    var name: String
    var id: Int
    var mToy: Toy
    
    enum CodingKeys: String, CodingKey {
        case id = "emplyeeID"
        case name
        case mToy
    }
}

struct Toy: Codable {
    var name:String
}

let employee = Employee(name: "你好", id: 123, mToy: Toy(name: "小玩意"))

do {
    let data = try JSONEncoder().encode(employee)
    let jsonStr = String(data: data, encoding: .utf8)!
    print(jsonStr)
} catch {
    print("发生错误")
}

```

`jsonString` 的输出结果为  

```json
{"name":"你好","mToy":{"name":"小玩意"},"emplyeeID":123}
```

说明：

1. `CodingKeys` 必须是嵌套在声明的 `struct` 中。

2. `CodingKeys` 必须遵守 `CodingKey` 协议。

3. 因为键都是 `String` 类型，所以需要在 `CodingKeys` 上继承 `String`，即：

   ```swift
   enum CodingKeys: String, CodingKey
   ```

4. 即使不打算重新命名所有的键也要在 `CodingKeys` 中列出所有的键。