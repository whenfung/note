---
title: MBProgressHUD
---

讲解 `MBProgressHUD` 主要是为了在 `swift` 项目中使用 `objc` 库。

- 建立 `ProjectName_Bridge_Header.h` 头文件，`ProjectName` 是你项目（`TARGET`）的名称。

- 在项目 `TARGETS` 的 `build settings` 中找到 `Swift Compiler - General`，添加 `ProjectName_Bridge_Header.h` 的路径。

- 使用 `cocopods` 导入 `MBProgressHUD`

- 然后在 `ProjectName_Bridge_Header.h` 中引入 `MBProgressHUD` 的头文件

  ```objc
  #ifndef PDCode_Bridge_Header_h
  #define PDCode_Bridge_Header_h
  
  #import "MBProgressHUD.h"
  
  #endif /* PDCode_Bridge_Header_h */
  ```

  

  

