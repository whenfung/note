---
title: delete storyboard
---

一个 iOS 程序员变为成熟的标志之一就是不再使用故事板。步骤如下：

- 删除 `main.storyboard` 文件。
- 在 TARGETS 中，清空 Main Interface 输入框。
- 在 `Info.plist` 中， 删除 Storyboard Name 一项。（在 Application Scene Manifest 的子栏目中）

完成上面三个步骤之后，我们可以开始写代码了。

iOS 13 后，`AppDelegate.swift` 中的方法变少了，因为窗口管理的方法移到了 `SceneDelegate.swift` 中，`AppDelegate.swift` 只负责 App 生命周期。

所以我们需要在 `SceneDelegate.swift` 设置我们的窗口。

```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
    // Use this method to optionally configure and attach the UIWindow `window` to the provided UIWindowScene `scene`.
    // If using a storyboard, the `window` property will automatically be initialized and attached to the scene.
    // This delegate does not imply the connecting scene or session are new (see `application:configurationForConnectingSceneSession` instead).
    guard let windowScene = (scene as? UIWindowScene) else { return }
    
    window = UIWindow(frame: windowScene.coordinateSpace.bounds)
    window?.windowScene = windowScene
    
    // 下面两个语句的顺序不能反
    // 因为 makeKeyAndVisible 保证了有 keyWindow，而 ViewController 的 init 函数中很可能会用用到 keyWindow
    window?.makeKeyAndVisible()
    window?.rootViewController = ViewController()
}
```

## 兼容

有些项目需要兼容低于 iOS 13 以下的版本，则需要在 `AppDelegate.swift` 中实现下面的方法：

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

    if #available(iOS 13, *) {
        // do nothing
    } else {
        self.window = UIWindow()
        self.window!.makeKeyAndVisible()
        let vc = UIStoryboard.viewController(LoginVC.self, name: "Login")
        self.window!.rootViewController = vc
        self.window!.backgroundColor = .red
    }
    return true
}
```



