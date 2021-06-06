---
title: status bar
---

状态栏，是 iPhone 界面的重要的一个组件，基本上你能在每一个界面都看到状态栏。但是状态栏的样式设置绝对是一个坑。

状态栏的设置分为两种情况：没有导航栏的情况、有导航栏的情况。

系统中的状态栏只能修改其字体颜色，背景颜色默认是透明的，如果有导航栏，那么状态栏的背景颜色和导航栏的 `tintColor` 一致。

## 1. view controller

在没有导航栏的情况下，也就是普通的 view controller，设置状态栏很简单，使用如下函数即可。

```swift
override var preferredStatusBarStyle: UIStatusBarStyle {
    return .lightContent
}
```

如果你想要随时修改颜色，例如夜间模式，则需要做一些自定义，例如我们定义一个布尔变量 `isBlack` 来标记状态栏字体是否是黑色，然后进行改造，得到：

```swift
override var preferredStatusBarStyle: UIStatusBarStyle {
    return isBlack ? .darkContent : .lightContent
}
```

然后我们每次改动 `isBlack` 然后再调用该函数即可，系统不允许直接调用该函数，我们需要调用 `setNeedsStatusBarAppearanceUpdate()` 来更新颜色，例如我们可以添加点击事件来变色。

```swift
@IBAction func changeColor(_ sender: Any) {
    if isBlack {
        isBlack = false
        UIView.animate(withDuration: 1.0) {
            self.view.backgroundColor = UIColor.darkGray
            self.setNeedsStatusBarAppearanceUpdate()
        }
    } else {
        isBlack = true
        UIView.animate(withDuration: 1.0) {
            self.view.backgroundColor = UIColor.white
            self.setNeedsStatusBarAppearanceUpdate()
        }
    }
}
```

这里我添加了 ` UIView.animate` 动画效果，因此变色不会显得僵硬，会有个渐变的效果。

## 2. navigation controller

如果你使用了 `UINavigationController` 来管理所有的页面，则在页面内使用 `preferredStatusBarStyle` 并不能改变状态栏的颜色，因为此时状态栏的颜色由 `UINavigationController` 的 `preferredStatusBarStyle` 管理，所以我们可以创建 `UINavigationController` 的子类，然后绑定在故事板相应页面，根据不同的顶层控制器显示不同的颜色。

```swift
override var preferredStatusBarStyle: UIStatusBarStyle {
    if topViewController is ChangeAvatarVC {
        return .lightContent
    }
    return .default
}
```

当然你也可以直接使用下面语句，将状态栏字体改为白色。

```swift
// 状态栏字体为白色，状态栏和导航栏背景为黑色
navigationController?.navigationBar.barStyle = .black
```

还有一个问题就是当我们隐藏导航栏的时候，状态栏的背景就会变成透明，所以我们可以添加一个图层放到状态栏下方，形成背景色的效果，代码如下：

```swift
// 修改状态栏背景颜色
let window = UIApplication.shared.windows.filter {$0.isKeyWindow}.first
let statusBar = UIView(frame: (window?.windowScene?.statusBarManager?.statusBarFrame)!)
statusBar.backgroundColor = .white
window?.addSubview(statusBar)
```

此时状态栏的颜色彻底和导航栏独立了，自由度也就更大了。