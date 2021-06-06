---
title: cocopods
---

CocoaPods 是 Swift 和 Objective-C Cocoa 项目的依赖项管理器，它可以帮助您优雅地扩展项目。所有操作来自官网 <https://cocoapods.org>。

# 安装

```bash
sudo gem install cocoapods
```

## 使用

在你的 Xcode 项目目录下生成 `Podfile` 文件。

⚠️ CocoaPods 提供 `pod init` 命令智能生成 `Podfile`，你应该使用该命令。

编辑 `Podfile` 文件，格式如下：

```bash
# Uncomment the next line to define a global platform for your project
platform :ios, '9.0'

target 'FilterNumberIniOS' do
  # Comment the next line if you don't want to use dynamic frameworks
  use_frameworks!

  # Pods for FilterNumberIniOS
  pod 'RxSwift', '~> 5'
  pod 'RxCocoa', '~> 5'

end
```

然后执行 `pod install` 安装依赖，安装完后会在项目中生成 `App.xcworkspace` 文件。

使用 `open App.xcworkspace` 打开项目。

紧接着可以使用相应的库了，例如 `import Alamofire`。

## 库的更新

有时候我们依赖的第三方库更新了，为了同步第三方库，我们需要进行相应版本的更新，步骤如下：

- 修改 Podfile 中对应第三方库的版本
- 对应目录中打开命令行，执行 `pod update` 即可。

## 自制第三方库

- `git tag <version>`
- `git push --tags`
- 修改 `spec` 文件的 `tag` 和 `version`
- `pod trunk push`

