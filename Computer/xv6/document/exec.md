---
title: 9. 第一个系统调用 exec
---

`initcode.S` 干的第一件事是触发 `exec` 系统调用。就像我们在第 0 章看到的一样，`exec` 用一个新的程序来代替当前进程的内存和寄存器，但是其文件描述符、进程 id 和父进程都是不变的。

[initcode.S](https://github.com/professordeng/xv6-expansion/blob/master/initcode.S) 刚开始会将 `$argv，$init，$0` 三个值推入栈中，接下来把 `%eax` 设置为 `SYS_exec` 然后执行 `int T_SYSCALL`：这样做是告诉内核运行 `exec` 这个系统调用。如果运行正常的话，`exec` 不会返回：它会运行名为 `$init` 的程序，`$init` 是一个以空字符结尾的字符串，即 [/init](https://github.com/professordeng/xv6-expansion/blob/master/initcode.S#L23)。如果 `exec` 失败并且返回了，`initcode` 会循环调用一个不会返回的系统调用 `exit` 。

系统调用 `exec` 的参数是 `$init、$argv`。最后的 0 让这个手动构建的系统调用看起来就像普通的系统调用一样，我们会在第 3 章详细讨论这个问题。和之前的代码一样，XV6 努力避免为第一个进程的运行单独写一段代码，而是尽量使用通用于普通操作的代码。

第 2 章讲了 `exec` 的具体实现，概括地讲，它会从文件中获取的 `/init` 的二进制代码代替 `initcode` 的代码。现在 `initcode` 已经执行完了，进程将要运行 `/init`。 [init](https://github.com/professordeng/xv6-expansion/blob/master/init.c) 会在需要的情况下创建一个新的控制台设备文件，然后把它作为描述符 0，1，2 打开。接下来它将不断循环，开启控制台 shell，处理没有父进程的僵尸进程，直到 shell 退出，然后再反复。系统就这样运行起来了。
