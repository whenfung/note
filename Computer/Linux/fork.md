---
title: fork 函数
---

linux 中现有的进程可以利用 fork 函数创建子进程。

## 函数原型

pid_t fork(void);

pid_t 是一个宏定义，其实质是 int 被定义在 sys/types.h 的头文件中。如果创建子进程成功函数将会返回两个值，子进程返回 0，父进程返回子进程 ID；如果出错返回 -1。

linux 下可以通过 `man fork` 查看详细手册。

## 函数说明

一个现有进程可以调用 fork 函数创建一个新进程。由fork创建的新进程被称为子进程（child process）。fork 函数被调用一次但返回两次。两次返回的唯一区别是子进程中返回 0 而父进程中返回子进程ID。

子进程是父进程的副本，它将获得父进程数据空间、堆、栈等资源的副本。注意，子进程持有的是上述存储空间的“副本”，这意味着父子进程间不共享这些存储空间。

UNIX 将复制父进程的地址空间内容给子进程，因此，子进程有了独立的地址空间。在不同的 UNIX (Like) 系统下，我们无法确定 fork 之后是子进程先运行还是父进程先运行，这依赖于系统的实现。所以在移植代码的时候我们不应该对此作出任何的假设。

## 为什么 fork 会返回两次

由于在复制时复制了父进程的堆栈段，所以两个进程都停留在fork函数中，等待返回。因此fork函数会返回两次，一次是在父进程中返回，另一次是在子进程中返回，这两次的返回值是不一样的。过程如下图。

![fork](/operating-system/img/fork.png)

fork调用的一个奇妙之处就是它仅仅被调用一次，却能够返回两次，它可能有三种不同的返回值：

1. 在父进程中，fork返回新创建子进程的进程ID；
2. 在子进程中，fork返回0；
3. 如果出现错误，fork返回一个负值。

在 fork 函数执行完毕后，如果创建新进程成功，则出现两个进程，一个是子进程，一个是父进程。在子进程中，fork 函数返回0，在父进程中，fork 返回新创建子进程的进程 ID。我们可以通过 fork 返回的值来判断当前进程是子进程还是父进程。

其实就相当于链表，多个进程形成了链表，父进程的 fork 函数返回的值指向子进程的进程 id, 因为子进程没有子进程，所以其fork 函数返回的值为 0。

调用 fork 之后，数据、堆、栈有两份，代码仍然为一份但是这个代码段成为两个进程的共享代码段都从fork函数中返回，箭头表示各自的执行处。当父子进程有一个想要修改数据或者堆栈时，两个进程真正分裂。

## fork 示例

``` c
#include <stdio.h>      // printf
#include <stdlib.h>     // exit
#include <sys/wait.h>   // wait
#include <unistd.h>     // fork

int main()
{
	pid_t pid = fork();
	if(pid < 0) {
		perror("fork failed");
        }
	if (pid == 0) {
	        printf("I am the child with pid %d\n", (int) getpid());
		exit(42);
	}
	//We must be the parent
	int status = 0;	
	pid_t childpid = wait(&status);
	printf("I am the parent with pid %d : my child %d finished with status %d.\n", (int)getpid(), (int)childpid, WEXITSTATUS(status));     
	return 0;
}

```

WEXITSTATUS 截取 status 变量的低八位，也许是 wait 函数里只会重写参数的低八位吧。
fork() 在 Linux 系统中的返回值是没有 NULL 的，EAGAIN 表示进程数达到上限，ENOMEN 表示空间不足。

fork 的另一个特性是所有由父进程打开的描述符都被复制到子进程中。父、子进程中相同编号的[文件描述在内核中指向同一个 file 结构体，也就是说，file 结构体的引用计数要增加。

## fork 炸弹

既然是资源复制，我们就可以利用循环创建无数进程，耗尽硬件资源，这就是 fork 炸弹

```c
while(1){
    fork();
}
```

## 僵尸进程

如果子进程运行结束后，其父进程没有回收该子进程的进程控制块，那么该子进程就会变成僵尸进程，解决僵尸进程的方法有两种

1. 重启电脑
2. 杀掉僵尸进程的父进程

如何查找僵尸进程？

利用 `ps` 指令查看，利用 `grep` 过滤

```bash
ps -aux | grep defunct
```

