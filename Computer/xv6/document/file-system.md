---
title: 5. 文件系统
---

XV6 文件系统提供文件和目录，文件就是一个简单的字节数组，而目录包含指向文件和其他目录的引用。XV6 把目录实现为一种特殊的文件。目录是一棵树，它的根节点是一个特殊的目录 `root`。`/a/b/c` 指向一个在目录 `b` 中的文件 `c`，而 b 本身又是在目录 `a` 中的，`a` 又是处在 `root` 目录下的。不从 `/` 开始的目录表示的是相对调用进程当前目录的目录，调用进程的当前目录可以通过 `chdir` 这个系统调用进行改变。下面的这些代码都打开同一个文件（假设所有涉及到的目录都是存在的）。

~~~ C
chdir("/a");
chdir("b");
open("c", O_RDONLY);

open("/a/b/c", O_RDONLY);
~~~

第一个代码段将当前目录切换到 `/a/b`；第二个代码片段则对当前目录不做任何改变。

有很多的系统调用可以创建一个新的文件或者目录：`mkdir` 创建一个新的目录，`open` 加上 `O_CREATE` 标志打开一个新的文件，`mknod` 创建一个新的设备文件。下面这个例子说明了这 3 种调用：

~~~ C
mkdir("/dir");
fd = open("/dir/file", O_CREATE|O_WRONGLY);
close(fd);
mknod("/console", 1, 1);
~~~

`mknod` 在文件系统中创建一个文件，但是这个文件没有任何内容。相反，这个文件的元信息标志它是一个设备文件，并且记录主设备号和辅设备号（`mknod` 的两个参数），这两个设备号唯一确定一个内核设备。当一个进程之后打开这个文件的时候，内核将读、写的系统调用转发到内核设备的实现上，而不是传递给文件系统。

`fstat` 可以获取一个文件描述符指向的文件的信息。它填充一个名为 `stat` 的结构体，它在 `stat.h` 中定义为：

~~~ c
#define T_DIR  1
#define T_FILE 2
#define T_DEV  3
// Directory
// File
// Device
struct stat {
  short type;  // Type of file
  int dev;     // File system’s disk device
  uint ino;    // Inode number
  short nlink; // Number of links to file
  uint size;   // Size of file in bytes
};
~~~

文件名和这个文件本身是有很大的区别。同一个文件（称为 `inode`）可能有多个名字，称为 "连接"（links）。系统调用 `link` 创建另一个文件系统的名称，它指向同一个 `inode`。下面的代码创建了一个既叫做 `a` 又叫做 `b` 的新文件。

~~~ c
open("a", O_CREATE|O_WRONGLY);
link("a", "b");
~~~

读写 `a` 就相当于读写 `b`。每一个 `inode` 都由一个唯一的 `inode` 号直接确定。在上面这段代码中，我们可以通过 `fstat` 知道 `a` 和 `b` 都指向同样的内容：`a` 和 `b` 都会返回同样的 `inode` 号（`ino`），并且 `nlink` 数会设置为 2。

系统调用 `unlink` 从文件系统移除一个文件名。一个文件的 `inode` 和磁盘空间只有当它的链接数变为 0 的时候才会被清空，也就是没有一个文件再指向它。因此在上面的代码最后加上

`unlink("a")`，

我们同样可以通过 `b` 访问到它。另外，

~~~ C
fd = open("/tmp/xyz", O_CREATE|O_RDWR);
unlink("/tmp/xyz");
~~~

是创建一个临时 `inode` 的最佳方式，这个 `inode` 会在进程关闭 `fd` 或者退出的时候被清空。

XV6 关于文件系统的操作都被实现为用户程序，诸如 `mkdir`，`ln`，`rm` 等等。这种设计允许任何人都可以通过用户命令拓展 shell 。现在看起来这种设计是很显然的，但是 Unix 时代的其他系统的设计都将这样的命令内置在了 shell 中，而 shell 又是内置在内核中的。

有一个例外，那就是 [cd](https://github.com/professordeng/xv6-expansion/blob/master/sh.c#L160)，它是在 shell 中实现的。`cd` 必须改变 shell 自身的当前工作目录。如果 `cd` 作为一个普通命令执行，那么 shell 就会 `fork` 一个子进程，而子进程会运行 `cd`，`cd` 只会改变子进程的当前工作目录。父进程的工作目录保持原样。
