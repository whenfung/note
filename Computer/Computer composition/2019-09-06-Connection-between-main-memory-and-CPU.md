---
title: 3.4 主存储器与 CPU 的连接
date: 2019-11-14
---

## 1. 连接原理

1. 主存储器通过数据总线、地址总线和控制总线与 CPU 连接。
2. 数据总线的位数与工作频率的乘积正比于数据传输率。
3. 地址总线的位数决定了可寻址的最大内存空间
4. 控制总线（读/写）指出总线周期的类型和本次输入/输出操作完成的时刻

## 2. 主存容器的拓展

由于单个存储芯片的容量是有限的，它在字数或字长方面与实际存储器的要求都有差距，因此需要在字和位两方面进行扩充才能满足实际存储器的容量要求。通常采用位扩展法、字扩展法和字位同时扩展法来拓展主存容量。

### 2.1 位拓展法

CPU 的数据线数与存储芯片的数据位数不一定相等，此时必须对存储芯片扩位（即进行位扩展，用多个存储器件对字长进行扩充，增加存储字长），使其数据位数与 CPU 的数据线数相等。

位扩展的连接方式是将多个存储芯片的地址端、片选端和读写控制端相应并联，数据端分别引出。

用 8 片 8K×1 位的RAM 芯片组成 8K×8 位的存储器时，8片 RAM 芯片的地址线并联在地址总线上（即收到的地址信号是相同的），每片的数据线则依次作为 CPU 数据线的一位。（P104）

注意：仅采用位扩展时，各芯片连接地址线的方式相同，但连接数据线的方式不同，在某一时刻选中所有的芯片，所以片选信号 `CS` 要连接到所有芯片。

### 2.2 字拓展法

字扩展是指增加存储器中字的数量，而位数不变。字扩展将芯片的地址线、数据线、读写控制线相应并联，而由片选信号来区分各芯片的地址范围。

用 4 片 16K×8 位的存储器。4 片 RAM 芯片的数据线都和 CPU 的 `WE` 连在一起（复用）。需要一个译码器进行片选，这里需要 2 根线确定芯片。假设两根线为 A1A2，则歌芯片的地址分配如下（P104）

```markdown
第 1 片，最低地址：00 00000000000000；最高地址：00 11111111111111（16 位）
第 2 片，最低地址：01 00000000000000；最高地址：01 11111111111111
第 3 片，最低地址：10 00000000000000；最高地址：10 11111111111111
第 4 片，最低地址：11 00000000000000；最高地址：11 11111111111111
```

注意：仅采用字扩展时，各芯片连接地址线的方式相同，连接数据线的方式也相同，但在某一时刻只需选中部分芯片，所以通过片选信号 `CS` 或采用译码器设计连接到相应的芯片。

### 2.3 字位同时拓展法

实际上，存储器往往需要同时扩充字和位。字位同时扩展是既增加存储字的数量，又增加存储字长。

用 8 片 16K×4 位的 RAM 芯片组成 64K×8 位的存储器。每两片构成一组 16K×8 位的存储器（位拓展），4 组便构成  64K×8 位的存储器（字拓展）。地址线 A1A2 经译码器得到 4 个片选信号，A1A2 = 00 时，选中第一组芯片；A1A2 = 01 时，输出端 1 有效，选中第二组的芯片，以此类推。

注意：采用字位同时扩展时，各芯片连接地址线的方式相同，但连接数据线的方式不同，而且需要通过片选信号 `CS` 或采用译码器设计连接到相应的芯片。

## 3. 存储芯片的地址分配和片选

CPU 要实现对存储单元的访问，首先要选中存储芯片，既进行片选；然后为选中的芯片依地址选择相应的存储单元，以进行数据的存取，即进行字选。片内的字选通常是由 CPU 送出的 N 条低位地址线完成的，地址线直接接到所有存储芯片的地址输入端（N 由片内存储容量 `2^N` 决定）。片选信号的产生分为线选法和译码片选法。

### 3.1 线选法

线选法用除片内寻址外的高位地址线直接（或经反相器）分别接至各个存储芯片的片选端，当某地址线信息为 `0` 时，就选中与之对应的存储芯片。这些片选地址线每次寻址时只能有一位有效，不允许同时有多位有效，这样才能保证每次只选中一个芯片（或芯片组）。假设 4 片 2K×8 位存储芯片用线选法构成 8K×8 位存储器，各芯片的片选信号见下表，其中低位地址线 A₁₀~A₀ 作为字选线，用于片内寻址。

| 芯片 | A₁₄~A₁₁ |
| ---- | ------- |
| 0#   | 1110    |
| 1#   | 1101    |
| 2#   | 1011    |
| 3#   | 0111    |

优点：不需要地址译码器，线路简单。缺点：地址空间不连续，选片的地址线必须分时为低电平（否则不能工作），不能充分利用系统的存储器空间，造成地址资源的浪费。

### 3.2 译码片选法

译码片选法用除片内寻址外的高位地址线通过地址译码器芯片产生片选信号。如用 8 片 8K×8 位的存储芯片组成 64K×8 位存储器（地址线为 16 位，数据线为 8 位），需要 8 各片选信号；若采用线选法，除去片内寻址的 13 位地址线，仅余高 3 位，不足以产生 8 个片选信号。因此，采用译码片选法，即用一片 `74LS138` 作为地址译码器，则 `A15A14A13=000` 时选中第一片，`A15A14A13=001` 时选中第二片，以此类推（即 3 位二进制编码）。

## 4. 存储器与 CPU 的连接

### 4.1 合理选择存储芯片

要组成一个主存系统，选择存储芯片是第一步，主要指存储芯片的类型（RAM 或 ROM）和数量的选择。通常选用 ROM 存放系统程序、标准子程序和各类常数，RAM 则是为用户编程而设置的。此外，在考虑芯片数量时，要尽量使连线简单、方便。

### 4.2 地址线的连接

存储芯片的容量不同，其地址线数也不同，而 CPU 的地址线数往往比存储芯片的地址线数要多。通常将 CPU 地址线的低位与存储芯片的地址线相连，以选择芯片中的某一单元（字选），这部分的译码是由芯片的片内逻辑完成的。而 CPU 地址线的高位则在扩充存储芯片时使用，用来选择存储芯片（片选），这部分译码由外接译码器逻辑完成。

例如，设 CPU 地址线为 16 位，即 A₁₅~A₀，1K×4 位的存储芯片仅有 10 根地址线，此时可将 CPU 的低位地址 A₉~A₀ 与存储芯片的地址线 A₉~A₀ 相连。

### 4.3 数据线的连接

CPU 的数据线数与存储芯片的数据线数不一定相等，在相等时可直接相连；在不等时必须对存储芯片扩位，使其数据位数与 CPU 的数据线数相等。

### 4.4 读/写命令线的连接

CPU 读/写命令线一般可直接与存储芯片的读/写控制端相连，通常高电平为读，低电平为写。有些 CPU 的读/写命令线是分开的（读为 `RD`，写为 `WE` ，均为低电平有效），此时 CPU 的读命令线应与存储芯片的允许读控制端相连，而 CPU 的写命令线则应与存储芯片的允许写控制端相连。

### 4.5 片选线的连接

片选线的连接是 CPU 与存储芯片连接的关键。存储器由许多存储芯片叠加而成，哪一片被选中完全取决于该存储芯片的片选控制端 `CS` 是否能接收到来自 CPU 的片选有效信号。

片选有效信号与 CPU 的访存控制信号 `MREQ` （低电平有效）有关，因为只有当 CPU 要求访存时才要求选中存储芯片。若 CPU 访问 I/O，则 `MREQ` 为高，表示不要求存储器工作。

## 5. 习题

1. MAR 的位数确定方法？
2. 字拓展和位拓展是啥？
3. 译码器的输入地址如何确定？

## 6. 习题答案

1. MAR 是地址寄存器，应该保证能访问到整个主存地址空间，和实际物理空间无关。主存地址空间是 CPU 的设计确定的。
2. 1024×4 中，4 代表一个存储单元只有 4 位，如果 CPU 数据宽度为 8 位，需要两个芯片进行 4+4 位拓展，字拓展 1024 个字不够，拓展地址长度。
3. 假设地址单元总长为 n，芯片地址总长为 x，则低 x 位就是片内寻址，而从 x+1 位开始就是移码器的输入地址了，输入线的个数得看芯片的多少（例如芯片 8 个，输入线得有 3 条，就需要最高的 3 位二进制作为译码器的输入地址） 。

