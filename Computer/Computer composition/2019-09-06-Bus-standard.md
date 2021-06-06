---
title: 6.4 总线标准
date: 2019-12-02
---

总线标准是国际上公布或推荐的**互连各个模块的标准**，是把各种不同的模块组成计算机系统时必须遵守的规范。按总线标准设计的接口可视为**通用接口**，在接口的两端，任何一方只需根据总线标准的要求完成自身方面的功能要求，而无须了解对方接口的要求。

## 1. 常见的总线标准

目前，典型的总线标准有 ISA、EISA、VESA、PCI、PCI-Express、AGP、RS-232C、USB 等。它们的主要区别是**总线宽度**、**带宽**、**时钟频率**、**寻址能力**、**是否支持突发传送**等。

1. ISA。ISA（Industry Standard Architecture，工业标准体系结构）总线是最早出现的微型计算机的**系统总线**，应用在 IBM 的 AT 机上。
2. EISA。EISA（Extended Industry Standard Architecture，扩展的 ISA）总线是为配合 32 位 CPU 而设计的**扩展总线**，EISA 对 ISA 完全兼容。
3. VESA。VESA（Video Electronics Standard Association，视频电子标准协会）总线是一个 32 位标准的计算机**局部总线**，是针对多媒体 PC 要求高速传送活动图像的大量数据应运而生的。
4. PCI。PCI（Peripheral Component Interconnect，外部设备互连）总线是高性能的 32 位或 64 位总线，是专为高度集成的外围部件、扩充插板和处理器/存储器系统设计的互连机制。目前常用的 PCI 适配器有**显卡**、**声卡**、**网卡**等。PCI 总线支持即插即用。PCI 总线是一个与处理器时钟频率无关的高速外围总线，属于**局部总线**。PCI 总线可通过**桥**连接实现多层 PCI 总线。
5. PCI-Express（PCI-E）。PCI-Express 是最新的总线和接口标准，它将全面取代现行的 PCI 和 AGP，最终**统一总线标准**。
6. AGP。AGP（Accelerated Graphics Port，加速图形接口）是一种视频接口标准，专用于**连接主存和图形存储器**，属于局部总线。AGP 技术位传输视频和三维图形数据提供了切实可行的解决方案。
7. RS-232C。RS-232C（Recommended Standard，RS）是由美国电子工业协会（EIA）推荐的一种串行通信总线，是应用于串行二进制交换的数据终端设备（DTE）和数据通信设备（DCE）之间的标准接口。
8. USB。USB（Universal Serial Bus，通用**串行总线**）是一种**连接外部设备**的 I/O 总线，属于设备总线。具有**即插即用**、**热拔插**等优点，有很强的连接能力。
9. PCMCIA。PCMCIA（Personal Computer Memory Card International Association）是广泛应用于笔记本的一种接口标准，是一个**用于扩展功能的小型插槽**。PCMCIA 具有**即插即用**功能。
10. IDE。IDE（Integrated Drive Electronics，集成设备电路），更准确地称为 ATA，是一种 IDE 接口**磁盘驱动器接口类型**，硬盘和光驱通过 IDE 接口与主板连接。
11. SCSI。SCSI（Small Computer System Interface，小型计算机系统接口）是一种用于计算机和智能设备之间（硬盘、软驱、光驱、打印机等）系统级接口的独立处理器标准。SCSI 是一种**智能的通用接口标准**。
12. SATA。SAT（Serial Advanced Technology Attachment，串行高级技术附件）是一种基于行业标准的**串行硬件驱动器接口**，是由 Intel、IBM、Dell、APT、Maxtor 和 Seagate 公司共同提出的硬盘接口规范。

## 2. 习题

1. 记住所有总线标准的缩写。
2. USB 的特点
3. 局部总线有
4. 现代微机采用局部总线技术的作用是

## 3. 习题答案

1. CPI 是每条指令的时钟周期数、RAM 是半导体随机存储器、CRT 是纯平显示器。不要混记
2. 串行、连接外部设备、即插即用、热拔插
3. VESA、AGP、PCI
4. 高速设备采用局部总线连接，节省系统的总带宽（显卡、声卡、视频处理、加速图形等之类的）。