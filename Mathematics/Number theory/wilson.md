---
title: Wilson 定理
---

## 作业

1. 证明：n 是素数的充要条件是：

   1. $n \mid (n-1)!+1$

      必要性：n 是素数，由 `wilson` 定理知道 $(n-1)!\equiv-1(mod \ n)$ ，因此 $(n-1)!+1\equiv 0(mod \ n)$ 。

      充分性：假设 n 不是素数，则 $n \mid (n-1)!$ ，和 $n\mid (n+1)!+1$ 矛盾

## 实例

1. 设 $r_0,r_1,\cdots,r_{p-1}$ 及 $r_0',r_1',\cdots,r_{p-1}'$ 是莫 p 的两组完全剩余系，p 是奇素数，证明

   $r_0r_0',r_1r_1',\cdots r_{p-1}r_{p-1}'$ 一定不是模 p 的完全剩余系。

2. 设 p 是奇素数，证明：

   $1^2\cdot 3^2\cdots(p-2)^2\equiv(-1)^{(p+1)/2}(mod \ p)$

## 背景知识

1. $wilson$ 定理

   设 p 是素数，$r_1,\cdots,r_{p-1}$ 是模 p 的既约剩余系，我们有

   $\prod\limits_{r \ mod \ p}'r\equiv r_1\cdots r_{p-1}\equiv-1(mod \ p)$

   特别地有

   $(p-1)!\equiv -1(mod \ p)$

2. 设素数 $p\ge3,l\ge1,c=\phi(p^l)$ ，以及 $r_1,r_2,\cdots,r_c$ 是模 $p^l$ 的一组既约剩余系，我们有

   $r_1\cdot r_2\cdots r_c \equiv -1 (mod p^l)$

   特别地有

   $\prod\limits_{r=1}^{p-1}\prod\limits_{s=0}^{p^{l-1}-1}(r+ps)\equiv -1(mod \ p^l)$

3. 设素数 $p\ge 3,l\ge1,c=\phi(2p^l)$ ，以及 $r_1,\cdots,r_l$ 是模 $2p^l$ 的一组既约剩余系，我们有

   $r_1\cdot r_2\cdots r_c\equiv-1(mod \ 2p^l)$

4. 设 $c=\phi(2^l)=2^{l-1},l\ge1,r_1,\cdots,r_c$ 是模 $2^l$ 的既约剩余系，我们有
   $$
   r_1\cdots r_c\equiv
   \begin{cases}
   -1(mod \ 2^l),l=1,2; \\
   1(mod \ 2^l),l\ge 3
   \end{cases}
   $$
   

{{site.math}}