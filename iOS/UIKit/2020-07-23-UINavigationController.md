---
title: UINavigationController
---

多页面必须有相应的导航，不然用户会懵的。

- `navigation bar` 的大标题可以在故事板中设置

```swift
navigationController?.navigationBar.prefersLargeTitles = false
```

- 主题颜色：在 `Assets.xcassets` 中添加 `New Color Set`

```swift
navigationController?.navigationBar.titleTextAttributes = [
	NSAttributedString.Key.foregroundColor : UIColor(named: "theme")!
]
```

- 标题属性 ：`titleTextAttributes`，是一个字典，使用 ` NSAttributedString.Key` 中的属性设置

- 单个页面导航栏背景透明

```swift
navigationController?.navigationBar.setBackgroundImage(UIImage(), for: .default)
navigationController?.navigationBar.shadowImage = UIImage()
```

- 整个 UINavigationController 背景透明，在 UINavigationController 中设置

```swift
// Make navigation bar background transparent
self.navigationBar.setBackgroundImage(UIImage(), for: .default)
self.navigationBar.shadowImage = UIImage()
```

- 内容顶到导航栏，不给导航栏留位置 ：故事板中 table view 的 content insets 设置为 never。

- 返回按钮的标题为空，只有一个返回图标 : 在导航栏的故事板中的 back button 设置为一个空格即可。 

- 滑动时隐藏导航栏，选中故事板中的导航控制器，属性中设置 Hide Bars 的 `on swipe` 属性。

- `translucent` ：将渐变效果去掉，不然你的导航栏颜色比你设置的要浅一点（个人感觉渐变很不协调）。

- 去掉导航栏返回按钮的标题。

   ```swift
   navigationItem.backBarButtonItem = UIBarButtonItem(title: "", style: .plain, target: nil, action: nil)
   ```

- 状态栏颜色（也可以在 storyboard 中设置）

```swift
navigationController?.navigationBar.barStyle = .black
```

在没有导航栏的情况下，可以直接覆盖下面的函数：

```swift
override var preferredStatusBarStyle: UIStatusBarStyle {
    return .lightContent
}
```

- 获取根视图

```swift
let window = UIApplication.shared.windows.filter {$0.isKeyWindow}.first     
let rootViewController = window?.rootViewController
```

- 让状态栏颜色和导航栏一样

```swift
// 这个方法是设置导航栏背景颜色，状态栏也会随之变色
[self.navigationController.navigationBar setBarTintColor:[UIColor redColor]];
```

- Push 切换控制器时发生卡顿，那是因为没有设置第二个控制器的 view 的背景色，使用下面语句即可：

```swift
let secondVC = UIViewController()
secondVC.view.backgroundColor = .white
navigationController?.pushViewController(secondVC, animated: true)
```

## 转场和反向转场

- 转场：系统会提供如下方法，可供你在转场前做一些设置。

```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    if segue.identifier == "showAreaDetail" {
        let dest = segue.destination as! DetailTableViewController
        dest.area = areas[tableView.indexPathForSelectedRow!.row]
    }
}
```

- 代码实现跳转

```swift
let storyboard = UIStoryboard(name: "Main", bundle: nil)
let fpc = storyboard.instantiateViewController(withIdentifier: "ForgetPasswordController")
self.navigationController?.pushViewController(fpc, animated: true)
```

### 反向转场

如果使用故事板的话，方法如下：

1. 在目标控制器中定义一个方法：此方法只有一个参数，类型必须是 `UIStoryboardSegue`，且有修饰符 `@IBAction`，例如

   ```swift
   @IBAction func close(segue: UIStoryboardSegue) {
       // 仅返回即可，无需任何操作
   }
   ```

2. 中 Storyboard 上指定反向转场关联。

   ctrl 拖动 “关闭按钮” 至视图的 Exit（出口处），选择 `close`

如果使用闭包的话，方法如下：

场景描述：从 `SecondVC` 返回到 `FirstVC` 时，将 `UiTextField` 的值同步给 `FirstVC`

1. 在 `SecondVC` 中定义一个闭包：

   ```swift
   var getUsername: ((String) -> Void)? = nil
   ```

   闭包参数是 `String`，返回类型是 Void，我们在闭包里头将数据传给 `FirstVC`。

2. 在 `SecondVC` 中的 `viewWillDisappear` 中使用闭包。

   ```swift
   if let getUsername = getUsername {
       getUsername(usernameTextField.text ?? "")
   }
   ```

   这里就将数据传入了闭包。

3. 在 `FirstVC` 中合适的地方引用闭包，闭包的触发时机在 `viewWillDisappear` 中，我们在 `prepare` 方法中实现：

   ```swift
   vc.getUsername = { [weak self] identity in
   	self?.usernameTextField.text = identity
   }
   ```


## 隐藏本页导航栏

设置代理 ：`    self.navigationController?.delegate = self`

然后实现 `willShow viewController` 协议 ：

```swift
extension MineVC: UINavigationControllerDelegate {
    func navigationController(_ navigationController: UINavigationController, willShow viewController: UIViewController, animated: Bool) {
        if viewController is MineVC {
            navigationController.setNavigationBarHidden(true, animated: true)
        } else {
            navigationController.setNavigationBarHidden(false, animated: true)
        }
    }
}
```



