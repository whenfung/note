---
title: 虚拟内存
---

测试环境：1 核 2 G Ubuntu 18.04

## 1. 申请内存

物理内存只有 2 G，除去运行内核，只剩不到 1.9G，此时用 malloc 申请 2G，进程并不会崩。