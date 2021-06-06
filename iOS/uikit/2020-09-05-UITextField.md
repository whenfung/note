---
title: UITextField
---

# placeholder

修改 placeholder ：

```swift
let placeholserAttributes = [NSAttributedString.Key.foregroundColor : UIColor.black, NSAttributedString.Key.font : UIFont.systemFont(ofSize: 18)]
        textField.attributedPlaceholder = NSAttributedString(string: "请输入账号", attributes: placeholserAttributes)
```

## return 监听

```swift
func textFieldShouldReturn(_ textField: UITextField) -> Bool {
    switch textField {
    case input.identityInput:
        (input.codeInput ?? input.passwordInput).becomeFirstResponder()
    case input.codeInput:
        input.passwordInput.becomeFirstResponder()
    case input.passwordInput:
        return input.confirmButton.isEnabled
    default: break
    }
    return true
}
```

## 下划线

重写 `draw` 方法

```swift
override func draw(_ rect: CGRect) {
    // 获取绘图上下文
    guard let context = UIGraphicsGetCurrentContext() else {
        return
    }
    // 创建并设置路径
    let path = CGMutablePath()
    path.move(to: CGPoint(x: self.bounds.minX, y: self.bounds.maxY))
    path.addLine(to: CGPoint(x: self.bounds.maxX, y: self.bounds.maxY))
    // 添加路径到图形上下文
    context.addPath(path)
    // 设置笔触颜色
    context.setStrokeColor(UIColor.hexString("#EFEFEF").cgColor)
    // 设置笔触宽度
    context.setLineWidth(1.0)
    // 绘制路径
    context.strokePath()
}
```

