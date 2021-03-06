---
title: 7.3 IO 接口
date: 2019-12-07
---

I/O 接口（I/O 控制器）是主机和外设之间的交接界面，通过接口可以实现主机和外设之间的信息交换。主机和外设具有各自的工作特点，它们在信息形式和工作速度上具有很大的差异，接口正是为了解决这些差异而设置的。

## 1. I/O 接口的功能

I/O 接口的主要功能如下

1. 实现主机和外设的**通信联络控制**。解决主机与外设时序配合问题，协调不同工作速度的外设和主机之间交换信息，以保证整个计算机系统能统一、协调地工作。
2. 进行**地址译码**和**设备选择**。CPU 送来选择外设的地址码后，接口必须对地址进行译码以产生设备选择信息，使主机能和指定外设交换信息。
3. 实现**数据缓冲**。CPU 与外设之间的速度往往不匹配，为消除速度差异，接口必须设置数据缓冲寄存器，用于数据的暂存，以避免因速度不一致而丢失数据。
4. **信息格式的转换**。外设与主机两者的电平、数据格式都可能存在差异，接口应提供计算机与外设的信号格式的转换功能，如电平转换、并/串或串/并转换、模/数或数/模转换等。
5. **传送控制命令和状态信息**。CPU 要启动某一外设时，通过接口中的命令寄存器向外设发出**启动命令**；外设准备就绪时，则将 “准备好” 状态信息送回接口中的状态寄存器，并反馈给 CPU。外设向 CPU 提出**中断请求**和 **DMA 请求**时，CPU 也应有相应的**响应信号**反馈给外设。

## 2. I/O 接口的基本结构

为使 I/O 接口实现基本功能，需要具有相应的逻辑电路。（P273）

CPU 与外设之间的信息传送，实质上是对**接口中的某些寄存器**（即端口）进行读或写，如传送数据是对数据端口 DBR 进行读写操作。

内部接口：内部接口与系统总线相连，实质上是与内存、CPU 相连。数据的传输方式只能是并行传输。

外部接口：外部接口通过接口电缆与外设相连，外部接口的数据传输可能是串行方式，因此 I/O 接口需具有串/并转换功能。

## 3. I/O 接口的类型

从不同的角度看，I/O 接口可以分为不同的类型。

1. **按数据传送方式**可分为并行接口（一个字节或一个字的所有位同时传送）和串行接口（一位一位地传送），接口要完成数据格式的转换。

   注意：这里所说的数据传送方式指的是外设和接口一侧的传送方式，而**在主机和接口一侧，数据总是并行传送的**。

2. 按**主机访问 I/O 设备的控制方式**可分为程序查询接口、中断接口和 DMA 接口等。

3. **按功能选择的灵活性**可分为可编程接口和不可编程接口。

## 4. I/O 端口及其编址

I/O 端口是指接口电路中可被 CPU 直接访问的**寄存器**，主要有数据端口、状态端口和控制端口，**若干端口加上相应的控制逻辑电路组成接口**。通常，CPU 能对数据端口执行读写操作，但对状态端口**只能执行**读操作，对控制端口**只能执行**写操作。

I/O 端口要想能够被 CPU 访问，必须要有**端口地址**，每个端口对应一个端口地址。面对 I/O 端口的编址方式有与存储器统一编址和独立编址两种。

1. 统一编址，又称**存储器映射方式**，是指把 I/O 端口当作存储器的单元进行地址分配，这种方式 CPU 不需要设置专门的 I/O 指令，用**统一的访存指令**就可以访问 I/O 端口。

   优点：不需要专门的输入/输出指令，可使 CPU 访问 I/O 的操作更灵活、更方便，还可使端口有较大的编址空间。

   缺点：端口占用存储器地址，使**内存容量变小**，而且利用存储器编址的 I/O 设备进行数据输入/输出操作，**执行速度较慢**。

2. 独立编址，又称 I/O 映射方式，是指 I/O 端口地址与存储器地址无关，独立编址 CPU 需要**设置专门的输入/输出指令**访问端口。

   优点：**输入/输出指令**与存储器指令有明显区别，程序编制清晰，便于理解。

   缺点：输入/输出指令少，一般只能对端口进行传送操作，尤其需要 CPU 提供存储器读/写、I/O 设备读/写两组控制信号，增加了控制的复杂性。

## 5. 习题

1. IO 总线的数据线上传输的信息包括
2. 统一编址情况下，IO 地址的特点
3. 磁盘驱动器写入磁盘是并行还是串行？
4. 程序员访问设备使用
5. IO **指令**实现的数据传送通常发生在
6. 按照数据传送方式，接口可分为？

## 6. 习题答案

1. 命令字（通道发送给 CPU）、状态字（通道发送给 CPU）、中断类型号（设备发给 CPU 的中断号，用来找中断程序入口地址），因为这些都是 IO 接口发往 CPU 的。地址线和控制线都是单向的，从 CPU 传送给 IO 接口。
2. 要求相对固定在地址的某部分，更多固定在高端或低端，不能随意编址。
3. 串行
4. 逻辑地址
5. 通用寄存器和 IO 端口之间
6. 串行和并行传送方式，CPU 到接口一定是并行。

