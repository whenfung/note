---
title: 8. 运行第一个进程
---

现在第一个进程的状态已经被设置好了，让我们来运行它。在 `main` 调用了 `userinit` 之后， `mpmain` 调用 [scheduler](https://github.com/professordeng/xv6-expansion/blob/master/main.c#L57) 开始运行进程。[scheduler](https://github.com/professordeng/xv6-expansion/blob/master/proc.c#L314) 会找到一个 `p->state` 为 `RUNNABLE` 的进程 `initproc`，然后将 `per-cpu` 的变量 `proc` 指向该进程，接着调用 [switchuvm](https://github.com/professordeng/xv6-expansion/blob/master/vm.c#L155) 通知硬件开始使用目标进程的页表。注意，由于 `setupkvm` 使得所有的进程的页表都有一份相同的映射，指向内核的代码和数据，所以当内核运行时我们改变页表是没有问题的。`switchuvm` 同时还设置好任务状态段 `SEG_TSS`，让硬件在进程的内核栈中执行系统调用与中断。我们后面会研究任务状态段。

`scheduler` 接着把进程的 `p->state` 设置为 `RUNNING`，调用 [swtch](https://github.com/professordeng/xv6-expansion/blob/master/swtch.S#L10)，切换上下文到目标进程的内核线程中。`swtch` 会保存当前的寄存器，并把目标内核线程中保存的寄存器（`proc->context`）载入到 X86 的硬件寄存器中，其中也包括栈指针和指令指针。当前的上下文并非是进程的，而是一个特殊的 `per-cpu` 调度器的上下文。所以 `scheduler` 会让 `swtch` 把当前的硬件寄存器保存在 `per-cpu` 的存储（`cpu->scheduler`）中，而非进程的内核线程上下文中。我们将在第 5 章讨论 `swtch` 的细节。最后的 [ret](https://github.com/professordeng/xv6-expansion/blob/master/swtch.S#L29) 指令从栈中弹出目标进程的 `%eip`，从而结束上下文切换工作。现在处理器就运行在进程 `p` 的内核栈上了。

`allocproc` 通过把 `initproc` 的 `p->context->eip` 设置为 `forkret` 使得 `ret` 开始执行 `forkret` 的代码。第一次被使用（也就是这一次）时，[forkret](https://github.com/professordeng/xv6-expansion/blob/master/proc.c#L394) 会调用一些初始化函数。注意，我们不能在 `main` 中调用它们，因为它们必须在一个拥有自己的内核栈的普通进程中运行。接下来 `forkret` 返回。由于 `allocproc` 的设计，目前栈上在 `p->context` 之后即将被弹出的字是 `trapret`，因而接下来会运行 `trapret`，此时 `%esp` 保存着 `p->tf`。[trapret](https://github.com/professordeng/xv6-expansion/blob/master/trapasm.S#L25) 用弹出指令从 [trap frame](https://github.com/professordeng/xv6-expansion/blob/master/x86.h#L147) 中恢复寄存器，就像 `swtch` 对内核上下文的操作一样： `popal` 恢复通用寄存器，`popl` 恢复 `%gs，%fs，%es，%ds`。`addl` 跳过 `trapno` 和 `errcode` 两个数据，最后 `iret` 弹出 `%cs，%eip，%flags，%esp，%ss`。trap frame 的内容已经转移到 CPU 状态中，所以处理器会从 trap frame 中 `%eip` 的值继续执行。对于 `initproc` 来说，这个值就是虚拟地址 0，即 `initcode.S` 的第一个指令。

这时 `%eip` 和 `%esp` 的值为 0 和 4096，这是进程地址空间中的虚拟地址。处理器的分页硬件会把它们翻译为物理地址。`allocuvm` 为进程建立了页表，所以现在虚拟地址 0 会指向为该进程分配的物理地址处。`allocuvm` 还会设置标志位 `PTE_U` 来让分页硬件允许用户代码访问内存。`userinit` 设置了 `%cs` 的低位，使得进程的用户代码运行在 CPL = 3 的情况下，这意味着用户代码只能使用带有 `PTE_U` 设置的页，而且无法修改像 `%cr3` 这样的敏感的硬件寄存器。这样，处理器就受限只能使用自己的内存了。
