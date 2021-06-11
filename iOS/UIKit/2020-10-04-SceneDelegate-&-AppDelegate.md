---
title: SceneDelegate & AppDelegate
---

iOS 13 后，一个应用可以有多个窗口，每个窗口由一个 scene 管理。因此我们把生命周期分为 App 的生命周期和窗口的生命周期，其中 App 的生命周期由 AppDelegate 响应，而窗口的生命周期由 SceneDelegate 响应。

## SceneDelegate

 SceneDelegate 是用来管理窗口的，这是 iOS 13 中新增的多场景窗口管理的类。

在该类中我们可以实现相应的代理，在窗口的不同生命周期中做相应的事情，例如：

- 在窗口进入后台时做一些网络请求，例如数据同步、状态同步。
- 在窗口被销毁前释放一些内存资源。

```swift
import UIKit

class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?

    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        // Use this method to optionally configure and attach the UIWindow `window` to the provided UIWindowScene `scene`.
        // If using a storyboard, the `window` property will automatically be initialized and attached to the scene.
        // This delegate does not imply the connecting scene or session are new (see `application:configurationForConnectingSceneSession` instead).
        guard let _ = (scene as? UIWindowScene) else { return }
        
        print("scene 连接成功。打开 app 后会输出这句话。")
    }

    func sceneDidDisconnect(_ scene: UIScene) {
        // Called as the scene is being released by the system.
        // This occurs shortly after the scene enters the background, or when its session is discarded.
        // Release any resources associated with this scene that can be re-created the next time the scene connects.
        // The scene may re-connect later, as its session was not necessarily discarded (see `application:didDiscardSceneSessions` instead).
        print("scene 断开连接成功。杀死 app 后会输出这句话。")
    }

    func sceneDidBecomeActive(_ scene: UIScene) {
        // Called when the scene has moved from an inactive state to an active state.
        // Use this method to restart any tasks that were paused (or not yet started) when the scene was inactive.
        print("scene 变为活跃状态，失去焦点。下拉状态栏后会输出这句话。按 home 键后会输出这句话。")
    }

    func sceneWillResignActive(_ scene: UIScene) {
        // Called when the scene will move from an active state to an inactive state.
        // This may occur due to temporary interruptions (ex. an incoming phone call).
        print("scene 将放弃活跃状态，失去焦点。下拉状态栏后，再上拉菜单栏会输出这句话。按 home 键，再回到 app 会输出这句话。")
    }

    func sceneWillEnterForeground(_ scene: UIScene) {
        // Called as the scene transitions from the background to the foreground.
        // Use this method to undo the changes made on entering the background.
        print("scene 将进入前台，按 home 键后，再回来会输出这句话。")
    }

    func sceneDidEnterBackground(_ scene: UIScene) {
        // Called as the scene transitions from the foreground to the background.
        // Use this method to save data, release shared resources, and store enough scene-specific state information
        // to restore the scene back to its current state.
        print("scene 将进入后台。按 home 键后会输出这句话。")
    }

}
```

注意，`sceneDidDisconnect` 表示场景失去连接，但是并没有被销毁，我们可以重新连接该场景。

## AppDelegate

这个类主要追踪 App 的生命周期状态，并负责分配或释放 Scene。

```swift
@main
class AppDelegate: UIResponder, UIApplicationDelegate {

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        
        print("App 启动成功，可以在这里做一些初始化工作")
        return true
    }

    // MARK: UISceneSession Lifecycle

    func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration {
        // Called when a new scene session is being created.
        // Use this method to select a configuration to create the new scene with.
        
        print("检测到 Scene 创建成功，可以做一些配置")
        
        return UISceneConfiguration(name: "Default Configuration", sessionRole: connectingSceneSession.role)
    }

    func application(_ application: UIApplication, didDiscardSceneSessions sceneSessions: Set<UISceneSession>) {
        // Called when the user discards a scene session.
        // If any sessions were discarded while the application was not running, this will be called shortly after application:didFinishLaunchingWithOptions.
        // Use this method to release any resources that were specific to the discarded scenes, as they will not return.
        
        print("检测到 Scene 被销毁，可以对相应资源进行回收")
    }

}

```

可以注意到 `@main`，这是一个 App 运行的入口，而 `application(_:didFinishLaunchingWithOptions:)` 是 App 运行的第一个函数。

## 参考文献

- [Managing Your App's Life Cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)

