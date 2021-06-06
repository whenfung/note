---
title: linux /proc/ 文件系统下的进程信息
---

`Linux` 内核提供了一种通过` proc` 文件系统，在运行时访问内核内部数据结构、改变内核设置的机制。proc 文件系统是一个伪文件系统，它只存在内存当中，而不占用外存空间。它以文件系统的方式为访问系统内核数据的操作提供接口。

本文将利用 `linux` 下 的 `/proc` 文件系统对一个进程做彻底的分析。 

## 内部文件结构

 `cd /proc` 进入目录下，呈现出来的是以一堆数字命名的文件夹，这些数字都是进程号。还有一些文件，通过 `ll` 指令就可以知道。这些文件记录了很多信息。例如 `cpuinfo` 记录了当前机器的 CPU 信息，`meminfo` 记录了内存信息，你可以通过 `cat` 指令查看。

进程（Process）是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位，是操作系统结构的基础。若想查看 `linux` 下进程的各种信息，我们可以通过该文件系统得知。

## /proc/pid/filename

通过 `cat /proc/pid/filename` 查看进程信息，这里的 `pid` 是进程号，`filename` 是进程号目录下的文件。下面以一个 `helloworld-getchar.c` 程序为例进行分析。代码如下 

```c
#include <stdio.h>

int main(){
	printf("hello world!\n");
	getchar();
	return 0;
}
```

这里的 `getchar()` 函数是为了让进程不死，不让在 `/proc` 下就找不到了。执行后，打开另一个终端，利用 `ps -a` 查看进程号，有了进程号，我们继续做下面操作。

1. `cat /proc/pid/maps`

   显示进程的内存区域映射信息，我的可执行文件名为 `a.out` ，前三行都是程序数据，全部数据都是私有的，通过权限可以得出，三行分别表示代码区、只读区和变量区。接下来就是堆，用来拓展内存的。然后是一些动态链接库，（fc 个人认为是设备号），最后还可以看到栈，以及系统调用等等。

   ```bash
   5635fdfb5000-5635fdfb6000 r-xp 00000000 fc:01 1059207                    /home/ubuntu/linux/memory/a.out
   5635fe1b5000-5635fe1b6000 r--p 00000000 fc:01 1059207                    /home/ubuntu/linux/memory/a.out
   5635fe1b6000-5635fe1b7000 rw-p 00001000 fc:01 1059207                    /home/ubuntu/linux/memory/a.out
   5635fec95000-5635fecb6000 rw-p 00000000 00:00 0                          [heap]
   7f38dee25000-7f38df00c000 r-xp 00000000 fc:01 131621                     /lib/x86_64-linux-gnu/libc-2.27.so
   7f38df00c000-7f38df20c000 ---p 001e7000 fc:01 131621                     /lib/x86_64-linux-gnu/libc-2.27.so
   7f38df20c000-7f38df210000 r--p 001e7000 fc:01 131621                     /lib/x86_64-linux-gnu/libc-2.27.so
   7f38df210000-7f38df212000 rw-p 001eb000 fc:01 131621                     /lib/x86_64-linux-gnu/libc-2.27.so
   7f38df212000-7f38df216000 rw-p 00000000 00:00 0 
   7f38df216000-7f38df23d000 r-xp 00000000 fc:01 131597                     /lib/x86_64-linux-gnu/ld-2.27.so
   7f38df430000-7f38df432000 rw-p 00000000 00:00 0 
   7f38df43d000-7f38df43e000 r--p 00027000 fc:01 131597                     /lib/x86_64-linux-gnu/ld-2.27.so
   7f38df43e000-7f38df43f000 rw-p 00028000 fc:01 131597                     /lib/x86_64-linux-gnu/ld-2.27.so
   7f38df43f000-7f38df440000 rw-p 00000000 00:00 0 
   7ffea1c21000-7ffea1c42000 rw-p 00000000 00:00 0                          [stack]
   7ffea1d3e000-7ffea1d41000 r--p 00000000 00:00 0                          [vvar]
   7ffea1d41000-7ffea1d43000 r-xp 00000000 00:00 0                          [vdso]
   ffffffffff600000-ffffffffff601000 r-xp 00000000 00:00 0                  [vsyscall]
   ```

2. `cat /proc/pid/limits`

   显示当前进程的资源限制。

   ```bash
   Limit                     Soft Limit           Hard Limit           Units     
   Max cpu time              unlimited            unlimited            seconds   
   Max file size             unlimited            unlimited            bytes     
   Max data size             unlimited            unlimited            bytes     
   Max stack size            8388608              unlimited            bytes     
   Max core file size        0                    unlimited            bytes     
   Max resident set          unlimited            unlimited            bytes     
   Max processes             7102                 7102                 processes 
   Max open files            1024                 1048576              files     
   Max locked memory         16777216             16777216             bytes     
   Max address space         unlimited            unlimited            bytes     
   Max file locks            unlimited            unlimited            locks     
   Max pending signals       7102                 7102                 signals   
   Max msgqueue size         819200               819200               bytes     
   Max nice priority         0                    0                    
   Max realtime priority     0                    0                    
   Max realtime timeout      unlimited            unlimited            us   
   ```

   `soft limit` 表示 `kernel` 设置给资源的值， 表示 `Soft Limit` 的上限，而 `Units` 则为计量单元。

3. `cat /proc/pid/stack`

   显示当前进程的内核调用栈信息，只有内核编译时打开了`CONFIG_STACKTRACE`编译选项，才会生成这个文件。

   ```bash
   [<0>] wait_woken+0x43/0x80
   [<0>] n_tty_read+0x425/0x8e0
   [<0>] tty_read+0x8e/0xf0
   [<0>] __vfs_read+0x1b/0x40
   [<0>] vfs_read+0x8e/0x130
   [<0>] SyS_read+0x55/0xc0
   [<0>] do_syscall_64+0x73/0x130
   [<0>] entry_SYSCALL_64_after_hwframe+0x3d/0xa2
   [<0>] 0xffffffffffffffff
   ```

4. `cat /proc/pid/statm`

   显示进程所占用内存大小的统计信息，包含七个值，度量单位是`page`（`page`大小可通过`getconf PAGESIZE`得到）。

   ```bash
   1127 179 164 1 0 77 0
   ```

   总共有七个值，含义分别是

   1. 进程占用的总的内存；
   2. 进程当前时刻占用的物理内存；
   3. 同其它进程共享的内存；
   4. 进程的代码段；
   5. 共享库（从`2.6`版本起，这个值为`0`）；
   6. 进程的堆栈；
   7. `dirty pages`（从`2.6`版本起，这个值为`0`）。

5. `cat /proc/pid/status`

   进程的状态信息。很多内容与 `stat` 和 `statm` 重复，但是却是以一种更清晰地方式展现出来。

   ```bash
   Name:	a.out
   Umask:	0002
   State:	S (sleeping)
   Tgid:	17472
   Ngid:	0
   Pid:	17472
   PPid:	10654
   TracerPid:	0
   Uid:	500	500	500	500
   Gid:	500	500	500	500
   FDSize:	64
   Groups:	4 24 27 30 46 113 114 500 
   NStgid:	17472
   NSpid:	17472
   NSpgid:	17472
   NSsid:	10654
   VmPeak:	    4508 kB
   VmSize:	    4508 kB
   VmLck:	       0 kB
   VmPin:	       0 kB
   VmHWM:	     716 kB
   VmRSS:	     716 kB
   RssAnon:	      60 kB
   RssFile:	     656 kB
   RssShmem:	       0 kB
   VmData:	     176 kB
   VmStk:	     132 kB
   VmExe:	       4 kB
   VmLib:	    2112 kB
   VmPTE:	      52 kB
   VmSwap:	       0 kB
   HugetlbPages:	       0 kB
   CoreDumping:	0
   Threads:	1
   SigQ:	0/7102
   SigPnd:	0000000000000000
   ShdPnd:	0000000000000000
   SigBlk:	0000000000000000
   SigIgn:	0000000000000000
   SigCgt:	0000000000000000
   CapInh:	0000000000000000
   CapPrm:	0000000000000000
   CapEff:	0000000000000000
   CapBnd:	0000003fffffffff
   CapAmb:	0000000000000000
   NoNewPrivs:	0
   Seccomp:	0
   Speculation_Store_Bypass:	vulnerable
   Cpus_allowed:	1
   Cpus_allowed_list:	0
   Mems_allowed:	00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000001
   Mems_allowed_list:	0
   voluntary_ctxt_switches:	1
   nonvoluntary_ctxt_switches:	4
   ```

   这里的 VM 是虚拟内存空间，peak 是最大值，size 是当前大小，RSS是正在使用的物理内存的大小，swap 是交换空间。data 是初始化的数据段，`stk` 是用户态栈大小。PTE 是该进程的所有页表的大小，lib 是链接库的大小。

6. `cat /proc/pid/syscall`

   显示当前进程正在执行的系统调用。

   ```bash
   0 0x0 0x5635fec95670 0x400 0x5635fec95010 0x7f38df2128c0 0x7f38df4314c0 0x7ffea1c40ec8 0x7f38def35081
   ```

   第一个值是系统调用号（`0`代表`read`），后面跟着`6`个系统调用的参数值（位于寄存器中），最后两个值依次是堆栈指针和指令计数器的值。如果当前进程虽然阻塞，但阻塞函数并不是系统调用，则系统调用号的值为`-1`，后面只有堆栈指针和指令计数器的值。如果进程没有阻塞，则这个文件只有一个“`running`”的字符串。

   内核编译时打开了 `CONFIG_HAVE_ARCH_TRACEHOOK` 编译选项，才会生成这个文件。

   通过以下指令查看系统调用表

   ```bash
   locate unistd_64  # 或者 unistd_32
   ```

7. `cat /proc/pid/wchan`

   示当进程`sleep`时，`kernel`当前运行的函数。

   ```bash
   wait_woken
   ```

8. `hexdump -x /proc/pid/auxv`

   传递给进程的`ELF`解释器信息。

   ```bash
   0000000    0021    0000    0000    0000    1000    a1d4    7ffe    0000
   0000010    0010    0000    0000    0000    fbff    1f8b    0000    0000
   0000020    0006    0000    0000    0000    1000    0000    0000    0000
   0000030    0011    0000    0000    0000    0064    0000    0000    0000
   0000040    0003    0000    0000    0000    5040    fdfb    5635    0000
   0000050    0004    0000    0000    0000    0038    0000    0000    0000
   0000060    0005    0000    0000    0000    0009    0000    0000    0000
   0000070    0007    0000    0000    0000    6000    df21    7f38    0000
   0000080    0008    0000    0000    0000    0000    0000    0000    0000
   0000090    0009    0000    0000    0000    5580    fdfb    5635    0000
   00000a0    000b    0000    0000    0000    01f4    0000    0000    0000
   00000b0    000c    0000    0000    0000    01f4    0000    0000    0000
   00000c0    000d    0000    0000    0000    01f4    0000    0000    0000
   00000d0    000e    0000    0000    0000    01f4    0000    0000    0000
   00000e0    0017    0000    0000    0000    0000    0000    0000    0000
   00000f0    0019    0000    0000    0000    1229    a1c4    7ffe    0000
   0000100    001a    0000    0000    0000    0000    0000    0000    0000
   0000110    001f    0000    0000    0000    1ff0    a1c4    7ffe    0000
   0000120    000f    0000    0000    0000    1239    a1c4    7ffe    0000
   0000130    0000    0000    0000    0000    0000    0000    0000    0000
   0000140
   ```

9. `cat /proc/pid/cmdline`

   只读文件，包含进程的完整命令行信息。如果这个进程是`zombie`进程，则这个文件没有任何内容。

   ```bash
   ./a.out
   ```

   通过 `ps -ef | grep a.out` 确认。

10. `cat /proc/pid/comm`

    进程的命令名。

11. `cd cwd`

    `cwd` 就是个链接。

12. `strings environ`

    进程的环境变量。

    ```bash
    USER=ubuntu
    LOGNAME=ubuntu
    HOME=/home/ubuntu
    PATH=/home/ubuntu/gems/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
    MAIL=/var/mail/ubuntu
    SHELL=/usr/bin/zsh
    SSH_CLIENT=218.17.40.57 34814 22
    SSH_CONNECTION=218.17.40.57 34814 172.16.0.17 22
    SSH_TTY=/dev/pts/0
    TERM=xterm
    XDG_SESSION_ID=863
    XDG_RUNTIME_DIR=/run/user/500
    LANG=en_US.utf8
    SHLVL=1
    PWD=/home/ubuntu/linux/memory
    OLDPWD=/home/ubuntu/linux
    ZSH=/home/ubuntu/.oh-my-zsh
    UPDATE_ZSH_DAYS=13
    PAGER=less
    LESS=-R
    LC_CTYPE=en_US.utf8
    LSCOLORS=Gxfxcxdxbxegedabagacad
    ```

13. `ll exe`

    实际运行程序的符号链接。

14. `ll fd`

    一个目录，包含进程打开文件的情况。

    ```
    total 0
    lrwx------ 1 ubuntu ubuntu 64 May 14 16:27 0 -> /dev/pts/0
    lrwx------ 1 ubuntu ubuntu 64 May 14 16:27 1 -> /dev/pts/0
    lrwx------ 1 ubuntu ubuntu 64 May 14 16:27 2 -> /dev/pts/0
    ```





## 参考资料

[gun linux proc pid intro](<https://github.com/NanXiao/gnu-linux-proc-pid-intro#auxv>)