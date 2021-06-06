---
title: shell 常用指令
---

## 查找功能

在使用 Linux 时，经常需要进行文件查找。有两个主要命令：find 和 grep

- find 命令是根据**文件的属性**进行查找，如文件名，文件大小，所有者，所属组，是否为空，访问时间，修改时间等。 

  基本格式 : `find path expression`

  ```bash
  find / -name .vimrc # 从根目录开始查看文件名为 .vimrc 的文件
  ```

- grep是根据**文件的内容进行**查找，会对文件的每一行按照给定的模式(patter)进行匹配查找。

  ```bash
  grep -rn "hello,world!" * # 在当前目录下 查找"hello,world!"字符串
  
  * : 表示当前目录所有文件，也可以是某个文件名
  -r 是递归查找
  -n 是显示行号
  -R 查找所有文件包含子目录
  -i 忽略大小写
  ```

## man 指令

Linux提供了丰富的帮助手册，使用Linux man命令来查看一些不熟悉的命令的使用方法,还可以用来查询系统库文件中的一些函数定义和使用方法。如下查看 fork 的手册。man 有多个部分。

```bash
man fork
man 2 fork # 第二部分

1是普通的命令
2是系统调用,如open,write之类的(通过这个，至少可以很方便的查到调用这个函数，需要加什么头文件)
3是库函数,如printf,fread
4是特殊文件,也就是/dev下的各种设备文件
5是指文件的格式,比如passwd,就会说明这个文件中各个字段的含义
6是给游戏留的,由各个游戏自己定义
7是附件还有一些变量,比如向environ这种全局变量在这里就有说明
8是系统管理用的命令,这些命令只能由root使用,如ifconfig
```

## dd 指令

将源文件的数据读到目标文件中，可以用 `dd` 指令创建一个空白的文件。

创建一个 512MB 的 bean 文件，盘块大小为1K，/dev/zero 提供 null 字符流。

```bash
dd if=/dev/zero of=bean bs=1k count=512000 
```

将目标文件的数据读取到一个文件中用做分析。将 /dev/loop0 设备的数据从 5129 个盘块开始读取 261 个盘块，然后重定向到一个文件中。其中 `od` 指令会读取所给予的文件的内容，并将其内容以十六进制字码呈现出来。

```bash
dd if=/dev/loop0 bs=1k count=261 skip=5129 |od -tx1 -Ax > /tmp/dump_sp_hex 
```

## 利用 C 实现 shell

- [点击获取源码](https://github.com/professordeng/linux/blob/master/shell/shell.c)

