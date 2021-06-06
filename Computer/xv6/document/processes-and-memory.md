---
title: 2. 进程和内存
---

一个 XV6 进程由两部分组成，一部分是用户内存空间（指令，数据，栈），另一部分是仅对内核可见的进程状态。XV6 提供了分时特性：它在可用 CPU 之间不断切换，决定哪一个等待中的进程被执行。当一个进程不在执行时，XV6 保存它的 CPU 寄存器，当他们再次被执行时恢复这些寄存器的值。内核将每个进程和一个 `pid`（process identifier）关联起来。

一个进程可以通过系统调用 `fork` 来创建一个新的进程。`fork` 创建的新进程被称为 "子进程"，子进程的内存内容同创建它的进程（父进程）一样。`fork` 函数在父进程、子进程中都返回（一次调用两次返回）。对于父进程它返回子进程的 `pid`，对于子进程它返回 0。考虑下面这段代码：

~~~ c
int pid;
pid = fork();
if(pid > 0){
    printf("parent: child=%d\n", pid);
    pid = wait();
    printf("child %d is done\n", pid);
} else if(pid == 0){
    printf("child: exiting\n");
    exit();
} else {
    printf("fork error\n");
}
~~~

系统调用 `exit` 会导致调用它的进程停止运行，并且释放诸如内存和打开文件在内的资源。系统调用 `wait` 会返回一个当前进程已退出的子进程，如果没有子进程退出，`wait` 会等候直到有一个子进程退出。在上面的例子中，下面的两行输出

~~~bash
parent: child=1234
child: exiting
~~~

可能以任意顺序被打印，这种顺序由父进程或子进程谁先结束 `printf` 决定。当子进程退出时，父进程的 `wait` 也就返回了，于是父进程打印：

~~~
parent: child 1234 is done
~~~

需要留意的是父子进程拥有不同的内存空间和寄存器，改变一个进程中的变量不会影响另一个进程。

系统调用 `exec` 将从某个 "文件"（通常是可执行文件）里读取内存镜像，并将其替换到调用它的进程的内存空间。这份文件必须符合特定的格式，规定文件的哪一部分是指令，哪一部分是数据，哪里是指令的开始等等。XV6 使用 ELF 文件格式，之后会详细介绍它。当 `exec` 执行成功后，它并不返回到原来的调用进程，而是从 ELF 头中声明的入口开始，执行从文件中加载的指令。`exec` 接受两个参数：可执行文件名和一个字符串参数数组。举例来说：

~~~ c
char *argv[3];
argv[0] = "echo";
argv[1] = "hello";
argv[2] = 0;
exec("/bin/echo", argv);
printf("exec error\n");
~~~

这段代码将调用程序替换为 `/bin/echo` 这个程序，这个程序的参数列表为`echo hello`。大部分的程序都忽略第一个参数，这个参数惯例上是程序的名字（此例是 echo）。

XV6 shell 用以上调用为用户执行程序。shell 的主要结构很简单，详见 [main](https://github.com/professordeng/xv6-expansion/blob/master/sh.c#L144) 的代码。主循环通过 [getcmd](https://github.com/professordeng/xv6-expansion/blob/master/sh.c#L158) 读取命令行的输入，然后它调用 `fork` 生成一个 shell 进程的副本。父 shell 调用 `wait`，而子进程执行用户命令。举例来说，用户在命令行输入 “echo hello”，`getcmd` 会以 `echo hello` 为参数调用 [runcmd](https://github.com/professordeng/xv6-expansion/blob/master/sh.c#L167)，由 `runcmd` 执行实际的命令。对于 `echo hello`, `runcmd` 将调用 `exec` 。如果 `exec` 成功被调用，子进程就会转而去执行 `echo` 程序里的指令。在某个时刻 `echo` 会调用 `exit`，这会使得其父进程从 `wait` 返回。你可能会疑惑为什么 `fork` 和 `exec` 为什么没有被合并成一个调用，我们之后将会发现，将 ”创建进程“ 和 ”加载程序“ 分为两个过程是一个非常机智的设计。

XV6 通常隐式地分配用户的内存空间。`fork` 在子进程需要装入父进程的内存拷贝时分配空间，`exec` 在需要装入可执行文件时分配空间。一个进程在需要额外内存时可以通过调用 `sbrk(n)` 来增加 n 字节的数据内存。 `sbrk` 返回新的内存的地址。

XV6 没有用户这个概念当然更没有不同用户间的保护隔离措施。按照 Unix 的术语来说，所有的 XV6 进程都以 root 用户执行。

