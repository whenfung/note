---
title: storyboard
---

## 绑定

故事板可以让我们轻松构建 UI 界面。这里有一些小技巧。

1. 在 `show the identity inspector` 中，我们可以给某个页面绑定一个控制器，绑定之后注意 `class` 输入框右侧有个 ➡️，点击后可以直接进入相应的控制器，提高编程效率。

## 修改故事板入口

假设你要把主故事板从 `Main.storyboard` 改为 `Second.storyboard`，步骤如下：

- 新建 `Second.storyboard`
- 在 `Info.plist` 中修改 `Main storyboard file base name` 为 `Second`，修改 `Storyboard Name` 为 `Second`。
- 在 `TARGETS` 中将项目的 `Main Interface` 改为 `Second`（测试时发现改了这个同时会自动改 `Main storyboard file base name`）