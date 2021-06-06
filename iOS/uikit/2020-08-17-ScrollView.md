---
title: ScrollView
---

介绍如何将 UIScrollView 成功添加到故事板的某个控制器中。按着下面的步骤走：

- 添加 Scroll View 到 View Controller 中
  - 添加约束，上下左右都是 0
  - show the size inspector 中 **不勾选** Content Layout Guides
- 添加一个 View 到 Scroll View 内
  - 添加约束，上下左右都是 0
  - 添加约束，水平居中
  - 添加约束，高度为 1000

完成上面两步后，就没有冲突了，但是，如果要改变内容高度，必须在代码处实时修改内容高度。