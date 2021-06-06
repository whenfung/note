---
title: 2. 理论知识
---

1. 查看 OpenGL 版本

   首先得安装一个工具 

   ```bash
   sudo apt-get update && sudo apt-get install mesa-utils
   ```

   然后运行 `glxinfo | grep version` 查看。

2. OpenGL 一个最简单的窗口程序需要经过以下几个步骤

   - 初始化 `glfw`
   - 创建窗口
   - 创建视口
   - 初始化 glad
   - 页面死循环渲染交互
   - 回收退出

3. 渲染三角形步骤

   - 顶点着色器的编写
   - 片元着色器的编写
   - 链接着色器
   - 创建 VAO 对象，用来管理 VBO
   - 创建 VBO 对象，用来缓存顶点
   - 发送数据给 VBO
   - 告诉 GPU 关于 VBO 的布局
   - 激活着色器，绑定 VAO
   - 给我画
   
4. 正方形

   正方形等于两个三角形，三角形有重合的点，为了省空间，用索引对象 EBO 实现。