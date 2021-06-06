---
title: Linux 权限位详解
---

因为 Linux 是多用户操作系统，所以需要引入权限管理。

如果某目录文件出现 permission denied，直接键入

```bash
sudo chmod -R 777 xxx
```

意思是目录 `xxx` 下的所有文件，系统内的用户都可以使用。

## 1. 权限位解释

在 Linux 常用权限位如下：

1. 可读权限 ：r
2. 可写权限 ：w
3. 可执行权限 ：x
4. 粘滞位：t

### 1.1 可读权限

文件和目录的可读权限设置一样：

1. 用字符表示：r
2. 用八进制表示：4
3. 可以对读取文件里的内容

### 1.2 可写权限

文件和目录的可写权限设置一样：

1. 用字符表示：w
2. 用八进制表示：2
3. 可以对文件进行更改

### 1.3 可执行权限

对于文件，可写权限：

1. 用字符表示：x
2. 用八进制表示：1
3. 可以执行该文件（脚本或命令），若是目录可以 cd 进入

### 1.4 粘滞位

这是一个特殊的权限位，对于目录：

1. 用字符表示：t
2. 用八进制表示：1000

`/tmp` 目录就是使用了粘滞位 `t`，其作用是在该目录下创建文件或目录后，仅允许其作者（所有者）进行删除操作。其他用户无法删除。

## 2. 修改权限样例

如下例子：

```bash
$ echo hello > a.txt    # 生成 a.txt 文件，内容为 hello
$ ls -sh a.txt          # 查看文件大小
4.0K a.txt              # 说明文件系统的存储单位为 4K
$ ls -lh a.txt          # list 表示列表显示, human 表示文件大小带单位
-rw-rw-rw- 1 ubuntu ubuntu 6 May  2 22:09 a.txt # 6 表示文件内容为 6 字节
$ chmod u=--- a.txt     # 设置不可读不可写
$ ls -lh a.txt          # 查看信息
----rw-rw- 1 ubuntu ubuntu 6 May  2 22:09 a.txt
$ cat a.txt             # 尝试读取信息
cat: a.txt: Permission denied  # 不可读
$ chmod u=rw- a.txt     # 设置可读可写
$ ls -lh a.txt          # 查看是否可读
-rw-rw-rw- 1 ubuntu ubuntu 6 May  2 22:09 a.txt 
$ cat a.txt             
hello                   # 正常读写
```

`drwxrwxr-x` 有 10 个字符，含义分别是：

第 1 个字符表示目录（directory）

第 2~4 个字符分别表示属主（`u`）的读、写、执行权限。

第 5~7 个字符分别表示属组（`g`）的读、写、执行权限。

第 8~10 个字符分别表示其他人（`o`）的读、写、执行权限。

## 3. 掩码

`umask` 是一个内置命令，其作用是指定创建的文件或目录的默认权限。

使用不加任何参数的 `umask` 会打印出八进制的权限，有 4 位。

- 第 1 个数字表示：特殊权限位的八进制数
- 第 2 个数字表示：属主的八进制数的反掩码
- 第 3 个数字表示：属组的八进制数的反掩码
- 第 4 个数字表示：其他人的八进制数的反掩码

手动更改时，可使用八进制，也可以使用字符表示：

```bash
# mkdir test_dir
# touch test_txt
# ll       
total 4.0K
drwxrwxr-x 2 ubuntu ubuntu 4.0K Mar 11 16:50 test_dir
-rw-rw-r-- 1 ubuntu ubuntu    0 Mar 11 16:50 test_txt
# rm -rf *      
# umask 777
# umask    
0777
# mkdir test_dir
# touch test_txt
# ll
total 4.0K
d--------- 2 ubuntu ubuntu 4.0K Mar 11 16:52 test_dir
---------- 1 ubuntu ubuntu    0 Mar 11 16:52 test_txt
```

注意：若你的 `umask` 设置为 0000，后续创建文件的权限依然是 666。出于安全着想，执行权限必须手动添加。所以你会看到，目录权限为 777，而文件权限为 666。

