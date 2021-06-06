---
title: 11. 练习
---

1. 在 `swtch` 中设断点。用 `gdb` 的 `stepi` 单步调试返回到 `forkret` 的代码，然后使用 `gdb` 的 `finish` 继续执行到 `trapret`，然后再用 `stepi` 直到你进入虚拟地址 0 处的 `initicode`。

2. `KERNBASE` 会限制一个进程能使用的内存量，在一台有着 4 GB 内存的机器上，这可能会让人感到不悦。那么提高 `KERNBASE` 的值是否能让进程使用更多的内存呢？