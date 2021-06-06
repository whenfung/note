---
title: 6. 第一个进程
---

![figure1-3](https://raw.githubusercontent.com/wiki/professordeng/blog/xv6/1-3.png)

当 PC 开机时，它会初始化自己然后从磁盘中载入 boot loader 到内存并运行。然后，boot loader 把 XV6 内核从磁盘中载入并从 [entry](https://github.com/professordeng/xv6-expansion/blob/master/entry.S#L45) 开始运行。X86 的分页硬件在此时还没有开始工作；所以这时的虚拟地址是直接映射到物理地址上的。

boot loader 把 XV6 内核装载到物理地址 0x100000 处。之所以没有装载到内核指令和内核数据应该出现的 0x80100000，是因为小型机器上很可能没有这么大的物理内存。而之所以在 0x100000 而不是 0x0 则是因为地址 0xa0000 到 0x100000 是属于 IO 设备的。

为了让内核的剩余部分能够运行，`entry` 的代码设置了页表，将 0x80000000（称为 [KERNBASE](https://github.com/professordeng/xv6-expansion/blob/master/memlayout.h#L8)）开始的虚拟地址映射到物理地址 0x0 处。注意，页表经常会这样把两段不同的虚拟内存映射到相同的一段物理内存，我们将会看到更多类似的例子。

`entry` 中的页表的定义为 [entrypgdir[]](https://github.com/professordeng/xv6-expansion/blob/master/main.c#L102)。我们后面会讨论页表的细节，这里简单地说明一下，页表项 0 将虚拟地址 0:0x400000 映射到物理地址 0:0x400000。只要 `entry` 的代码还运行在内存的低地址处，我们就必须这样设置，但最后这个页表项是会被移除的。`entrypgdir` 将虚拟地址的 KERNBASE:KERNBASE+0x400000 映射到物理地址 0:0x400000。这个页表项将在 `entry` 的代码结束后被使用；它将内核指令和内核数据应该出现的高虚拟地址处映射到了 boot loader 实际将它们载入的低物理地址处。这个映射就限制内核的 ”指令 + 代码“ 必须在 4 MB 以内。

让我们回到 `entry` 中继续页表的设置工作，它将 `entrypgdir[]` 的物理地址载入到控制寄存器 `%cr3` 中。分页硬件必须知道 `entrypgdir` 的物理地址，因为此时它还不知道如何翻译虚拟地址；它也还没有页表。`entrypgdir` 这个符号指向内存的高地址处，但只要用宏 [V2P_WO](https://github.com/professordeng/xv6-expansion/blob/master/memlayout.h#L14) 减去 `KERNBASE` 便可以找到其物理地址。为了让分页硬件运行起来， XV6 会设置控制寄存器 `%cr0` 中的标志位 `CR0_PG`。

现在 `entry` 就要跳转到内核的 C 代码，并在内存的高地址中执行它了。首先它将栈指针 [%esp](https://github.com/professordeng/xv6-expansion/blob/master/entry.S#L59) 指向被用作栈的一段内存。所有的符号包括 `stack` 都在高地址，所以当低地址的映射被移除时，栈仍然是可用的。最后 `entry` 跳转到高地址的 `main` 代码中。我们必须使用间接跳转，否则汇编器会生成 PC 相关的直接跳转（PC-relative direct jump），而该跳转会运行在内存低地址处的 `main`。 `main` 不会返回，因为栈上并没有返回 PC 值。好了，现在内核已经运行在高地址处的函数 [main](https://github.com/professordeng/xv6-expansion/blob/master/main.c#L14) 中了。
