---
title: 一次同余方程
---

## 实例

1. 解同余方程 $589x\equiv1026 (mod \ 817)$
2. $21x\equiv 38(mod \ 117)$

## 背景

设 $m \nmid a$ ，讨论最简单的**模 m 的一次同余方程**

$ax\equiv b(mod \ m)\cdots (1)$

如果同余方程（1）有解 $x=x_1$ ，则有某个整数 $y_1$ 使得

$ax_1=b+my_1$

因此，（1）有解的必要条件是

$(a,m) \mid b\cdots (2)$

例如，同余方程

 $4x\equiv 2 (mod \ 8)$ 

一定无解，因为 $(4,8)=4 \nmid 2$，同余方程

$3x\equiv2(mod \ 8)$ 

满足条件（2），因为(3,8)=1，在模 8 的绝对最小完全剩余系中，仅有解 x=-2。

同余方程 $6x\equiv 2 (mod \ 8)$ 有解 $x=-1,3$ 。

1. 当 (a,m)=1 时，同余方程 （1）必有解，且其解数为 1。

2. 同余方程（1）有解的充要条件是式（2）成立，在有解时，它的解数等于 (a,m)，以及若 $x_0$ 是（1）的解，则它的 (a,m) 个解是

   $x\equiv x_0+\frac m {(a,m)} t(mod \ m)$

   $t=0,\cdots,(a,m)-1$



{{site.math}}