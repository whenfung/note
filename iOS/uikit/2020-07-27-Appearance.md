---
title: Appearance
---

iOS 的 UI 组件外观批量设置，可以使用 Appearance API 来定制大多数 UI 控件的外观，通过 appearance 代理机制来实现。

当你的 APP 有主题的时候，可以在 APP 启动的时候，就对主题样式进行批量设置，例如在整个 app 的入口 AppDelegate 的 `didFinishLaunchingWithOptions` 方法里加入：

```swift
// 设置导航栏颜色
UINavigationBar.appearance().barTintColor = UIColor(red: 242/255, green: 116/255, blue: 119/255, alpha: 1)
        
// 设置导航栏字体颜色
UINavigationBar.appearance().tintColor = UIColor.white
        
// 设置字体，字体可能不存在，字体名可以从 storyboard 中找
// 这里表示字体是 Avenir，样式是 Light（轻量）
if let barFont = UIFont(name: "Avenir-Light", size: 24.0) {
	UINavigationBar.appearance().titleTextAttributes = [NSAttributedString.Key.foregroundColor:UIColor.white, NSAttributedString.Key.font:barFont]
}
```

当然你也可以通过 storyboard 对导航栏进行设置。下面注意：

- `translucent` ：将渐变效果去掉，不然你的导航栏颜色比你设置的要浅一点（个人感觉渐变很不协调）。