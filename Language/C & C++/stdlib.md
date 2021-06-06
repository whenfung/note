---
title: stdlib
---

`stdlib` 是一个相当实用的 C 语言标准库，这里记录我最常用的一些函数。

## 1. 字符处理

在 shell 命令下，传入命令程序的都是字符串，而我们经常需要字符串和数字之间的切换，库中包含了如下几个函数：

```c
double atof(char *str);  // 字符串转双精度
int atoi(char *str);     // 字符串转整数
long atol(char *str);    // 字符串转长整数
```

## 2. 内存分配

内存的动态分配和释放函数也在这个库，原型如下：

```c
void* malloc(unsigned size);  // 分配 size 字节的内存段
void* free(void* p);          // 释放 p 所指的内存段
```



