---
title: NSAttributedString
---

NSAttributedString 可用于字体高亮、URL 点击等，不过分几种情况：

1. 如果使用 UILabel 的话，很多复杂的操作做不了
2. 如果想支持复杂操作，建议使用 UITextView

## UILabel

首先从故事板中拉取相应控件：

```swift
@IBOutlet weak var statementLabel: UILabel!
```

然后给某些字体添加高亮颜色：

```swift
let attrStr = NSMutableAttributedString(string: statementLabel.text!)
attrStr.addAttribute(NSAttributedString.Key.foregroundColor, value:UIColor(named: "36b59d")!, range:NSRange.init(location:9, length: 4))      
attrStr.addAttribute(NSAttributedString.Key.foregroundColor, value:UIColor(named: "36b59d")!, range:NSRange.init(location:16, length: 4))
statementLabel.attributedText = attrStr
```

## UITextView

修改链接的颜色：

```swift
textView.linkTextAttributes = [NSAttributedString.Key.foregroundColor : UIColor.orange]
```

将富文本颜色配置进行封装，然后使用链式调用来简化：

```swift
import UIKit

extension NSMutableAttributedString {
    // 链接
    func makeHyperlink(for path: String, as substring: String) -> NSMutableAttributedString {
        let nsString = NSString(string: string)
        let substringRange = nsString.range(of: substring)
        addAttribute(.link, value: path, range: substringRange)
        
        return self
    }
    
    // 颜色高亮
    func makeHighlight(as substring: String) -> NSMutableAttributedString {
        let nsString = NSString(string: string)
        let substringRange = nsString.range(of: substring)
        addAttribute(NSAttributedString.Key.foregroundColor, value: UIColor.orange, range: substringRange)
        return self
    }
    
}
```

完成上述的封装后，富文本的修改就变得异常简单：

- 配置 URL 链接，并自定义颜色

```swift
let text = textView.text ?? ""
textView.attributedText = NSMutableAttributedString.init(string: text).makeHyperlink(for: "https://professordeng.com", as: "隐私政策")
textView.linkTextAttributes = [NSAttributedString.Key.foregroundColor : UIColor.orange]
```

- 仅高亮颜色

```swift
let text = textView.text ?? ""
textView.attributedText = NSMutableAttributedString.init(string: text).makeHighlight(as: "隐私政策")
```

- 自定义点击行为

有时候我们点击 URL 时，希望在自己的 APP 内部做一些处理，所以可以使用 UITextView 的一些代理方法：

```swift
func textView(_ textView: UITextView, shouldInteractWith URL: URL, in characterRange: NSRange, interaction: UITextItemInteraction) -> Bool {
//  UIApplication.shared.open(URL)
    print("点击了 \(URL.absoluteString)")
    return false
}
```

- 保留相应的设置

需要注意的是，如果你设置了 `attributedText`，那么之前故事板里对文本的设置就会消失，所以我们可以这样做：

```swift
// 保存原有设置
let font = textView.font
let textColor = textView.textColor
// 添加富文本
textView.attributedText = NSMutableAttributedString.init(string: textView.text!).makeHyperlink(for: "https://professordeng.com", as: "隐私政策")
textView.linkTextAttributes = [NSAttributedString.Key.foregroundColor: UIColor.orange]
// 添加原有设置，居中也要重新设置
textView.textAlignment = .center
textView.font = font
textView.textColor = textColor
```

