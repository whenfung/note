---
title: enca
---

enca 是一个命令行工具，可在 macOS 下安装。

- 安装

  ```bash
  brew install enca
  ```

- 查看文件编码格式

  ```bash
  enca -L zh_CN filename
  ```

- 编码转换

  ```bash
  enca -L zh_CN -x UTF-8 filename
  ```

  