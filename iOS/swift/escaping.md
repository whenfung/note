---
title: escaping
---

如果一个闭包被作为一个参数传递给一个函数，并且在函数 `return` 之后才被唤起执行，那么这个闭包是逃逸闭包。并且这个闭包的参数是可以 “逃出” 这个函数体外的。

所有网络请求的函数，在完成调用请求后，直到响应返回，闭包才会被调用，所以这种类型的网络请求函数内等待响应的闭包就是逃逸闭包。这个类型的闭包，需要程序员手工加入一个 `@escaping` 标记才可以编译通过。

如下代码，展示了一个非逃逸闭包，和一个逃逸闭包。后者已经被标记了 `@escapings`：

```swift
import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        func syncRequest(callBack: () -> Void ) {
            callBack()
        }
        func asyncRequest( callBack: @escaping () -> Void ) {
            DispatchQueue.main.asyncAfter(deadline: .now() + 1.0, execute: {
                callBack()
            })
        }
        syncRequest(){
            print("callback")
        }
        
        asyncRequest(){
            print("delay 1s callback")
        }
        
        // Override point for customization after application launch.
        return true
    }

}
```

函数 `DispatchQueue.main.asyncAfter` 用来延时。此处延时 1 秒 再调用 `callback`，演示了一个逃逸闭包的效果。

闭包可能需要引用当前上下文的变量，因此当调用者完成后，如果标记了逃逸闭包，那么当前调用的上下文依然会保持。如果在该标记的地方没有标记的话，在编译期间就会报错，因为编译器知道你没有立即调用 `callback`。

`swift` 中闭包默认是不可逃逸的，好处就是编译器优化你的代码的性能和能力。如果编译器知道这个闭包是不可逃逸的，它可以关注内存管理的关键细节。

在不可逃逸闭包里可以放心地使用 `self` 关键字，因为这个闭包总是在函数 `return` 之前执行，你不需要去使用一个弱引用去引用 `self`。

---

