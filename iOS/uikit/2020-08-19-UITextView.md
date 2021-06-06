---
title: UITextView
---

UITextView 是个强大又常用的组件，所以开篇文章特地介绍一下。

## 1. 属性

- 居左、居中等方式。

```swift
textView.textAlignment = .center
```

- 左右不留边距

```swift
textView.textContainer.lineFragmentPadding = 0
```

- 上下不留边距

```swift
textView.textContainerInset = .zero
```

- 修改链接颜色

```swift
textView.linkTextAttributes = [NSAttributedString.Key.foregroundColor : UIColor.orange]
```

- 链接点击事件获取

首先得遵循 UITextViewDelegate 协议。

```swift
func textView(_ textView: UITextView, shouldInteractWith URL: URL, in characterRange: NSRange, interaction: UITextItemInteraction) -> Bool {
//  UIApplication.shared.open(URL)
    print("点击了 \(URL.absoluteString)")
    
    let storyboard = UIStoryboard(name: "Main", bundle: nil)
    let svc = storyboard.instantiateViewController(withIdentifier: "StatementViewController") as! StatementViewController
    svc.url = URL
    self.navigationController?.pushViewController(svc, animated: true)
    
    return false
}
```

