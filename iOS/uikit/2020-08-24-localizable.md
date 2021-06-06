---
title: localizable
---

有些 APP 是面向全世界用户的，所以开发的时候需要考虑 iOS 语言国际化。

其中 iOS 语言国际化分为两部分 ：项目内的语言国际化、APP 名字国际化设置。

## 1. 项目内的语言国际化

首先是配置文件，步骤如下：

1. 创建名为 Localizable 的项目，按如下顺序添加你所需要的语言：

   点击左侧 PROJECT ➡️ 点击 Info ➡️ 找到 Localizations 栏，点击 + 号 ➡️ 添加支持的语言（例如 English、Chinese, simplified）

2. 创建 strings file，命名为 `Localizable.strings` 

3. 点击 `Localizable.strings` 文件，在右侧栏的 show the file inspector，在 Localization 栏中点击 `Localize...` ➡️ 随便选择一个你添加的语言，点击 Localize 按钮确定 ➡️ 右侧会列出所有添加的语言，勾选你确定要用的语言即可，勾选的时候弹出提示框，点击User file 按钮即可

4. 此时点开 Localize.strings 文件左侧箭头展开，把每种语言的文件里面写上翻译好的 app 名字：

   在 `Localizable.strings(Chinese, Simplified)` 的格式如下：

   ```swift
   "你好，世界";
   ```

   在 `Localizable.strings(English)` 的格式如下：

   ```swift
   "你好，世界" = "Hello, world";
   ```

完成以上步骤后，我们就可以使用 key-value 的方式进行语言国际化了，在 `ViewController.swift` 中编写简单的例子如下：

```swift
let label = UILabel(frame: CGRect(x: 100, y: 100, width: 100, height: 100))
label.backgroundColor = UIColor.yellow
label.textColor = UIColor.red
label.font = UIFont.systemFont(ofSize: 15)
label.textAlignment = .center
label.text = NSLocalizedString("你好，世界", comment: "")
view.addSubview(label)
```

此时，如果系统语言是中文，则该标签显示的内容 `Localizable.strings(English)` 中的内容，如果系统语言是英语，则该标签显示的内容是 `Localizable.strings(Chinese, Simplified)`，如果应用没有设置相对应的语言，则显示 `Localizable.strings` 的内容。

## 2. app 名字国际化设置

1. 同创建 `Localizable.strings` 文件一样，创建一个 `InfoPlist.strings` 文件。

2. 点击 `InfoPlist.strings` 文件，在右侧栏的 show the file inspector，在 Localization 栏中点击 `Localize...` ➡️ 随便选择一个你添加的语言，点击 Localize 按钮确定 ➡️ 右侧会列出所有添加的语言，勾选你确定要用的语言即可，勾选的时候弹出提示框，点击User file 按钮即可。

3. 点开 `InfoPlist.strings` 文件左侧箭头展开，把每种语言的文件里面写上翻译好的 app 名字。

   在 `InfoPlist.strings(Chinese, Simplified)` 的格式如下：

   ```swift
   CFBundleName = "我的应用";
   ```

   在 `InfoPlist.strings(English)` 的格式如下：

   ```swift
   CFBundleName = "我的应用";
   ```

然后运行 App，你就可以看到名字的切换了。

 