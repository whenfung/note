---
title: 7. 创建第一个进程
---

在 `main` 初始化了一些设备和子系统后，它通过调用 [userinit](https://github.com/professordeng/xv6-expansion/blob/master/main.c#L36) 建立了第一个进程。`userinit` 首先调用 `allocproc`。[allocproc](https://github.com/professordeng/xv6-expansion/blob/master/proc.c#L68) 的工作是在页表中分配一个 `proc` 结构体，并初始化进程的状态，为其内核线程的运行做准备。

注意一点：`userinit` 仅仅在创建第一个进程时被调用，而 `allocproc` 创建每个进程时都会被调用。

`allocproc` 会在  `ptable` 中找到一个标记为 [UNUSED](https://github.com/professordeng/xv6-expansion/blob/master/proc.c#L81) 的槽位。当它找到这样一个未被使用的槽位后，`allocproc` 将其状态设置为 `EMBRYO`，使其被标记为被使用的并给这个进程一个独有的 [pid](https://github.com/professordeng/xv6-expansion/blob/master/proc.c#L90)。接下来，它尝试为进程的内核线程分配内核栈。如果分配失败了，`allocproc` 会把这个槽位的状态恢复为 `UNUSED` 并返回 0 以标记失败。

#### 1-3

![figure1-4](/XV6-document/img/1-4.png)

现在 `allocproc` 必须设置新进程的内核栈，`allocproc` 以巧妙的方式，使其既能在创建第一个进程时被使用，又能在 `fork` 操作时被使用。`allocproc` 为新进程设置好一个特别准备的内核栈和一系列内核寄存器，使得进程第一次运行时会 “返回” 到用户空间。准备好的内核栈就像图 [1-3](#1-3) 展示的那样。`allocproc` 通过设置返回程序计数器的值，使得新进程的内核线程首先运行在 `forkret` 的代码中，然后返回到 [trapret](https://github.com/professordeng/xv6-expansion/blob/master/proc.c#L108) 中运行。内核线程会从 `p->context` 中拷贝的内容开始运行。所以我们可以通过将 `p->context->eip` 指向 `forkret` 从而让内核线程从 [forkret](https://github.com/professordeng/xv6-expansion/blob/master/proc.c#L394) 的开头开始运行。这个函数会返回到那个时刻栈底的地址。[switch](https://github.com/professordeng/xv6-expansion/blob/master/swtch.S#L10) 的代码把栈指针指向 `p->context` 结尾。`allocproc` 又将 `p->context` 放在栈上，并在其上方放一个指向 `trapret` 的指针；这样运行完的 `forkret` 就会返回到 `trapret` 中了。 `trapret` 接着从栈顶恢复用户寄存器然后跳转到用户进程执行。这样的设置对于普通的 `fork` 和建立第一个进程都是适用的，虽然后一种情况进程会从用户空间的地址 0 处开始执行而非真正的从 `fork` 返回。

我们将会在后面内容看到，将控制权从用户转到内核是通过中断机制实现的，中断具体地说是系统调用、中断和异常。每当进程运行中要将控制权交给内核时，硬件和 XV6 的 `trap entry` 代码就会在进程的内核栈上保存用户寄存器。[userinit](https://github.com/professordeng/xv6-expansion/blob/master/proc.c#L134) 把值写在新建的栈的顶部，使之就像进程是通过中断进入内核的一样。所以用于从内核返回到用户代码的通用代码也就能适用于第一个进程。这些保存的值就构成了一个结构体 `struct trapframe`，其中保存的是用户寄存器。现在如图 [1-3](#1-3) 所示，进程的内核栈已经完全准备好了。

第一个进程会先运行一个小程序 [initcode.S](https://github.com/professordeng/xv6-expansion/blob/master/initcode.S)，于是进程需要找到物理内存来保存这段程序。程序不仅需要被拷贝到内存中，还需要页表来指向那段内存。

最初，`userinit` 调用 [setupkvm](https://github.com/professordeng/xv6-expansion/blob/master/proc.c#L129) 来为进程创建一个只映射了内核区的页表。我们后面会学习该函数的具体细节。总之，`setupkvm` 和 `userinit` 创建了图 [1-1](#1-1) 所示的地址空间。

第一个进程内存中的初始内容是汇编过的 `initcode.S`；作为建立进程内核区的一步，链接器将这段二进制代码嵌入内核中并定义两个特殊的符号：`_binary_initcode_start` 和 `_binary_initcode_size`，用于表示这段代码的位置和大小。然后，`userinit` 调用 [inituvm](https://github.com/professordeng/xv6-expansion/blob/master/vm.c#L180)，分配一页物理内存，将虚拟地址 0 映射到那一段内存，并把这段代码拷贝到那一页中。

接下来，`userinit` 把 [trapframe](https://github.com/professordeng/xv6-expansion/blob/master/x86.h#L147) 设置为用户模式：`%cs` 寄存器保存着一个段选择器，指向段 `SEG_UCODE` ，它处于特权级 `DPL_USER`（即在用户模式而非内核模式）。类似的，`%ds, %es, %ss` 的段选择器指向段 `SEG_UDATA` 并处于特权级 `DPL_USER`。`%eflags` 的 `FL_IF` 位被设置为允许硬件中断；我们将在后面回头看这段代码。

栈指针 `%esp` 被设为了进程的最大有效虚拟内存地址，即 `p->sz`。指令指针则指向初始化代码的入口点，即地址 0。

函数 `userinit` 把 `p->name` 设置为 `initcode`，这主要是为了方便调试。还要将 `p->cwd` 设置为进程当前的工作目录；我们将在后面回过头来查看 `namei` 的细节。

一旦进程初始化完毕，`userinit` 将 `p->state` 设置为 `RUNNABLE`，使进程能够被调度。
