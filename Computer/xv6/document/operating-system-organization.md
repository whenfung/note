---
title: 1. 操作系统组织
---

一个操作系统的关键要求是同时支持多个进程，例如，一个进程使用 `fork` 系统调用产生一个新进程。 操作系统必须让进程间分时共享电脑资源，例如，即使进程需求大于当前硬件资源，操作系统也要保证所有进程都能执行下去。操作系统也必须保证进程间的隔离，如果某个进程出现 bug 而崩溃，也不会影响那些不相干的进程。但是完全隔离也不行，因为多个进程也有可能有交互，管道就是一个例子。因此一个操作系统必须要满足三个要求：多路复用、隔离和交互。

本章概述操作系统是如何实现这三个要求的。有很多方式可以实现，但是我们专注于主流设计，也就是被很多 Unix 使用的 ”大内核“。

本章通过第一个进程的创建来解释 XV6 是如何开始运行的，让我们得以一窥 xv6 提供的各个抽象是如何实现和交互的。XV6 尽量复用了普通操作的代码来建立第一个进程，避免单独为其撰写代码。接下来的各小节中，我们将详细探索其中的奥秘。

XV6 可以运行在搭载 Intel 80386 及其之后（即 X86）处理器的 PC 上，因而许多底层功能（例如虚存的实现）是 X86 处理器专有的。本书假设读者已有些许在一些体系结构上进行机器级编程的经验。我们将在有关 X86 专有概念出现时，对其进行介绍。另外，附录 A 中简要地描述了 PC 平台的整体架构。
