---
title: UIAlertController
---

`UIAlertController` 是系统自带的弹出框，有两种样式，分别是 `alert` 和 `actionSheet`。需要注意的是：

- 取消类型的按钮最多有一个

- 弹出框

```swift
let ac = UIAlertController(title: "提示", message: "您点击了按钮！", preferredStyle: .alert)

let btn = UIAlertAction(title: "好", style: .default, handler: nil)

ac.addAction(btn)

present(ac, animated: true, completion:nil)
```

- 选项类型：分为三种
  - `.default` ：普通按钮
  - `.cancel`：加粗字体按钮
  - `.destructive` ：红色字体按钮