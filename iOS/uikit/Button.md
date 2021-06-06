`swift` 学习可以使用 playground 快速学习。

```swift
import UIKit

let button = UIButton(frame: CGRect(x: 0, y: 0, width: 200, height: 50))

button.setTitle("立即订购", for: .normal)

button.setTitleColor(UIColor.white, for: .normal)
button.backgroundColor = .gray

// layer 是图层
button.layer.cornerRadius = 10
// 裁剪边角
button.clipsToBounds = true

button
```

## 1. 颜色

通过故事板的设置可以修改按钮的颜色，在属性设置的 view 栏中，background 是图片的背景颜色，tintcolor 是图片的前景颜色（其实就是字体之类的颜色）。**注意** ：要将 type 设置为 Syetem 时，tintcolor 才会生效。 

## 2. 运行时属性

在故事板中，你可以设置运行时的属性，例如圆角半径，在 `show the identity inspector` 中添加 `key path` 为 `layer.cornerRadius`，`type` 设为 `Number`，`value` 设置为 20，这样就可以实现圆角半径为 20。

## 3. 设置左图标

```swift
setImage(image, for: .normal)
imageEdgeInsets = UIEdgeInsets(top:14, left: 14, bottom: 14, right: 14)
```

## 4. 扩大点击区域

判断 point 是否在区域内即可。

```swift
override func point(inside point: CGPoint, with event: UIEvent?) -> Bool {
    return  bounds.inset(by: UIEdgeInsets(top: -5, left: -5, bottom: -5, right: -5)).contains(point)
}
```

## 5. 去除上下边距

UIButton 默认有个上下边距，大概为 6.0，可以使用代码去除

```swift
override func layoutSubviews() {
    // 缩小上下边距后按钮要往上移
    let Yoffset = (bounds.maxY - (titleLabel?.bounds.maxY)!) / 2
    frame.origin.y -= Yoffset
    
    bounds = titleLabel?.bounds ?? bounds
    titleLabel?.frame = titleLabel!.bounds
}
```

## 6. 背景图片做颜色

在 UIButton 的 extension 中实现下述函数：

```swift
func setBackgroundColor(color: UIColor, forState: UIControl.State) {
    self.clipsToBounds = true  // add this to maintain corner radius
    UIGraphicsBeginImageContext(CGSize(width: 1, height: 1))
    if let context = UIGraphicsGetCurrentContext() {
        context.setFillColor(color.cgColor)
        context.fill(CGRect(x: 0, y: 0, width: 1, height: 1))
        let colorImage = UIGraphicsGetImageFromCurrentImageContext()
        UIGraphicsEndImageContext()
        self.setBackgroundImage(colorImage, for: forState)
    }
}
```

然后可以简单调用：

```swift
button.setBackgroundColor(color: Colors.View.highlight, forState: .normal)
```

## 7. 苹果登录专用按钮

- 添加 Sign in with Apple 功能

依次选择 Xcode ➡️ Signing & Capabilities ➡️ + Capability，然后添加 Sign in with Apple 功能。

- 生成唯一的 [identitiers](https://developer.apple.com/account/resources/identifiers/list) 。

- 创建登录按钮

很不幸，故事板没有直接添加 苹果登录按钮的控件。需要引入相应的库

```swift
import AuthenticationServices
```

官方提供了一个 ASAuthorizationAppleIDButton （继承自 UIControl），使用这个来创建一个登录按钮。

## 8. 自定义苹果按钮

```swift
let appleButton = UIButton()

appleButton.setTitle("apple ID 登录", for: .normal)
appleButton.setTitleColor(.black, for: .normal)

appleButton.setImage(UIImage.init(named: "apple"), for: .normal)
appleButton.imageEdgeInsets = UIEdgeInsets(top: 0, left: 0, bottom: 0, right: 5)

appleButton.clipsToBounds = true
appleButton.layer.borderColor = UIColor.black.cgColor
appleButton.layer.borderWidth = 1
appleButton.layer.cornerRadius = 10

view.addSubview(appleButton)

appleButton.translatesAutoresizingMaskIntoConstraints = false
NSLayoutConstraint.activate([
    appleButton.centerXAnchor.constraint(equalTo: view.centerXAnchor),
    appleButton.centerYAnchor.constraint(equalTo: view.centerYAnchor),
    appleButton.heightAnchor.constraint(equalToConstant: 44),
    appleButton.widthAnchor.constraint(equalToConstant: 144)
])
```

其中，apple 图标尺寸为 `16*16px`。
