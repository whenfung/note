---
title: 3.1 存储器的层次结构
date: 2019-11-11
---

## 1. 存储器的分类

存储器种类繁多，可从不同角度对存储器进行分类。

### 1.1 按在计算机中的作用（层次）分类

1. 主存储器。简称主存，又称内存储器（内存），用来存放计算机运行期间所需的大量程序和数据，CPU 可以直接随时地对其进行访问，也可以和高速缓冲存储器（Cache）及辅助存储器交换数据。其特点是容量较小、存取速度较快、每位价格较高。
2. 辅助存储器。简称辅存，又称外存储器（外存），是主存储器的后援存储器，用来存放当前暂时不用的程序和数据，以及一些需要永久性保持的信息，它不能与 CPU 直接交换信息。其特点是容量极大、存取速度较慢、单位成本低。
3. 高速缓冲存储器。简称 Cache，位于主存和 CPU 之间，用来存放正在执行的程序段和数据，以便 CPU 能高速地使用它们。Cache 的存取速度可与 CPU 的速度相匹配，但存储容量小、价格高。目前的高档计算机通常将它们制作在 CPU 中。

### 1.2 按存储介质分类

按存储介质，存储器可分为磁表面存储器（磁盘、磁带）、磁心存储器半导体存储器（MOS 型存储器、双极型存储器）和光存储器（光盘）。

### 1.3 按存取方式分类

1. 随机存储器（RAM）。存储器的任何一个存储单位的内容都可以随机存取，而且存取时间与存储单位的物理位置无关。其优点是读写方便、使用灵活，主要用作主存或高速缓冲存储器。RAM 又分为静态 RAM（以触发器原理寄存信息）和动态 RAM（以电容充电原理寄存信息）。

2. 只读存储器（ROM）。存储器的内容只能随机读出而不能写入。信息一旦写入存储器就固定不变，即使断电，内容也不会丢失。因此，通常用它存放固定不变的程序、常数和汉字字库，甚至用于操作系统的固化。它与随机存储器可共同作为主存的一部分，统一构成主存的地址域。

   由 ROM 派生出的存储器也包含可反复重写的类型，ROM 和 RAM 的存取方式均为随机存取。注意广义上的只读存储器已可通过电擦除等方式进行写入，其 “只读” 的概念没有保留，但仍然保留了断电内容保留、随机读取特性，但其写入速度比读取速度慢得多。

3. 串行访问存储器。对存储单元进行读/写操作时，需按其物理位置的先后顺序寻址，包括顺序存取存储器（如磁带）与直接存取存储器（如磁盘）。

   顺序存取存储器的内容只能按某种顺序存取，存取时间的长短与信息在存储体上的物理位置有关，其特点是存取速度慢。直接存取存储器既不像 RAM 那样随机地访问任何一个存储单元，又不像顺序存取存储器那样完全按顺序存取，而是介于两者之间。存取信息时通常先寻找整个存储器中的某个小区域（如磁盘上的磁道），再在小区域内顺序查找。

### 1.4 按信息的可保存性分类

断电后，存储信息即消失的存储器，称为易失性存储器，如 RAM。断电后信息仍然保持的存储器，称为非易失性存储器，如 ROM、磁表面存储器和光存储器。若某个存储单元所存储的信息被读出时，原存储信息被破坏，则称为破坏性读出；若读出时，被读单元原存储信息不被破坏，则称为非破坏性读出。具有破坏性读出性能的存储器，每次读出操作后，必须紧接一个再生的操作，以便恢复被破坏的信息。

## 2. 存储器的性能指标

存储器有 3 个主要性能指标，即存储容量、单位成本和存储速度。这 3 个指标相互制约，设计存储器系统所追求的目标就是大容量、低成本和高速度。

1. 存储容量 = 存储字数×字长（如 1M×8 位）。单位换算：1B（Byte，字节）=8b（bit，位）。存储字数表示存储器的地址空间大小，字长表示一次存取操作的数据量。

2. 单位成本：每位价格 = 总成本/总容量。

3. 存储速度：数据传输率 = 数据的宽度/存储周期。（速度等于路程除以时间，数据的宽度就是一次存/取的数据量，存储周期就是执行一次存/取花费的时间）

   1. 存取时间（`Ta`）：存取时间是指从启动一次存储器操作到完成该操作所经历的时间，分为读出时间和写入时间。

   2. 存取周期（`Tm`）：存取周期又称读写周期或访问周期。它是指存储器进行一次完整的读写操作所需的全部时间，即连续两次独立访问存储器操作（读或写操作）之间所需的最小时间间隔

      ```matlab
      存取周期 = 存取时间 + 恢复时间
      ```

   3. 主存带宽（`Bm`）：主存带宽又称数据传输率，表示每秒从主存进出信息的最大数量，单位为字/秒，字节/秒（B/s）或位每秒（b/s）。

   存取时间不等于存储周期，通常存储周期大于存取时间。这是因为对任何一种存储器，在读写操作之后，总要有一段恢复内部状态的复原时间。对于破坏性读出的存储器，存取周期往往比存取时间大得多，甚至可达 `Tm=2Ta` ，因为存储器中的信息读出后需要马上进行再生。

## 3. 习题

1. EPROM、DRAM、SRAM、CD-ROM？
2. 磁盘属于哪类？
3. 注意是按字寻址还是按半字寻址
4. 相联存储器的寻址方式
5. 内存储器和外存储器分别用啥？

## 4. 习题答案

1. 闪存、动态 RAM、静态 RAM、光盘（靠物理旋转）

2. 直接存取（介于顺序和随机）

3. 按字：除以字的长度，按半字：除以半字的长度。

4. 即按内容又按地址

5. 内存储器：RAM、ROM

   外存储器：磁盘

