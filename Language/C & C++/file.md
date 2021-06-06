---
title: file
---

文件是计算机的资源之一，也是操作系统的重要概念之一。本节用 C 语言来读写文件。

## 1. 创建文件

源码在 `open.c`。

在 Linux 系统中，可以用 `open()` 来创建文件，其函数原型如下：

```c
int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode);
```

其中 `pathname` 是路径名，可以是相对路径，也可以是绝对路径。

`flags` 是打开的模式，包括可读、可写、创建等。

`mode` 是文件权限，如果权限认定当前用户不可读，即使 `flags` 是可读模式，也读不到文件的内容。

若 `open()` 调用成功，会返回代表文件的索引节点。在系统中，一个文件用一个索引节点表示。

## 2. 读写文件

在 Linux 中，读文件是用 `read()`，其函数原型为：

```c
ssize_t read(int fd, void *buf, size_t count);
```

写文件是用 `write()`，其函数原型为：

```c
ssize_t write(int fd, const void *buf, size_t count);
```

这里都是使用 `open()` 返回的文件描述符作为参数传入，至于能不能读到数据得看返回值是否大于 0。

## 3. 关闭文件

当不使用文件时，应该习惯性关闭文件，使用 `close()`，函数原型为：

```c
int close(int fd);
```

调用该函数后进程会释放 `fd`。