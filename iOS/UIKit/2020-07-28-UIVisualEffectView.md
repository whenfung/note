---
title: UIVisualEffectView
---

可以使用 `UIVisualEffectView` 对一个视图应用可视化特效。

配合 `UIBlurEffect` 类，可轻松添加一个背景虚化特效。

1. 创建一个特效视图，指定特效类型（有暗、亮、超亮）。
2. 设定特效视图的大小
3. 把特效视图叠加在原视图之上（原视图的子视图）

```swift
let effect = UIBlurEffect(style: .light)
let effectView = UIVisualEffectView(effect: effect)        
effectView.frame = view.frame
bgImageView.addSubview(effectView)
```

例如上述代码就可以实现对图片进行虚化。