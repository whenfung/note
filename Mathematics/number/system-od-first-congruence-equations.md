---
title: 一次同余方程组、孙子定理
---

## 实例

1. 解同余方程组
   $$
   \begin{cases}
   x\equiv1(mod \ 3), \\
   x\equiv -1(mod \ 5), \\
   x \equiv 2 (mod \ 7), \\
   x \equiv -2(mod \ 11)
   \end{cases}
   $$
解：$a_1=1,a_2=-1,a_3=2,a_4=-2$ ，
   
   $ m_1=3,m_2=-5,m_3=7,m_4=11$ ，
   
   $ m=m_1m_2m_3m_4=3\cdot5\cdot7\cdot11 $ ，
   
   $ M_1=5\cdot7\cdot11,M_2=3\cdot7\cdot11,M_3=3\cdot5\cdot11,M_4=3\cdot5\cdot7 $ ，
   
   $ M_1\equiv (-1)\cdot1\cdot(-1)\equiv 1(mod \ 3) ,M_1^{-1}=1 $ ，
   
   $ M_2\equiv(-2)\cdot2\cdot1\equiv1(mod \ 5),M_2^{-1}=1 $ ，
   
   $ M_3\equiv3\cdot5\cdot4\equiv4(mod \ 7),M_3^{-1}=2 $ ，
   
   $ M_4\equiv3\cdot5\cdot7\equiv6(mod \ 11),M_4^{-1}=2 $ ，
   
   $ x\equiv a_1M_1M_1^{-1}+\cdots+a_4M_4M_4^{-1}(mod \ m) $ 得
   
   $ x\equiv(5\cdot7\cdot11)\cdot1\cdot1+(3\cdot7\cdot11)\cdot1\cdot(-1)+(3\cdot5\cdot11)\cdot2\cdot2+(3\cdot5\cdot7)\cdot2\cdot(-2)(mod \ 3\cdot5\cdot7\cdot11)$ 即 $ x\equiv385-231+660-420\equiv394(mod \ 1155) $ 
   
2. 求相邻的四个整数，它们依次可被 $2^2,3^2,5^2$ 及 $7^2$ 整除。

   设这四个相邻整数是 $x-1,x,x+1,x+2$ ，按要求应该满足

   $x-1\equiv0(mod \ 2^2),x\equiv0(mod \ 3^2)$

   $x+1\equiv0(mod \ 5^2), x+2\equiv0(mod \ 7^2)$

   然后利用例子 1 的方法求解即可。

3. 求模 11 的一组完全剩余系，使其中每个数被 $2,3,5,7$ 除后的余数分别为 $1,-1,1,-1$ 。

   

4. 解同余方程组

   $x\equiv3(mod \ 8),x\equiv11(mod \ 20),x\equiv 1(mod \ 15)$

   因为 $m_1,m_2,m_3$ 不两两即约，因此不能使用孙子定理，拆分同余方程组得
   $$
   \begin{cases}
   x\equiv3(mod \ 8) \\
   x\equiv11(mod \ 4) \\
   x\equiv11(mod \ 5) \\
   x\equiv1(mod \ 5) \\
   x\equiv1(mod \ 3) 
   \end{cases}
   $$
   满足第一个方程一定满足第二个方程，第三个方程和第四个方程一样，得到
   $$
   \begin{cases}
   x\equiv3(mod \ 8) \\
   x\equiv1(mod \ 5) \\
   x\equiv1(mod \ 3) 
   \end{cases}
   $$
   剩余求解方法参照实例 1 。

5. 解同余方程 $19x\equiv 556(mod \ 1155)$

   $1155=3\cdot5\cdot7\cdot11$ ，拆分得到四个方程

   $19x\equiv556(mod \ 3),19x\equiv556(mod \ 5)$

   $19x\equiv556(mod \ 7),19x\equiv556(mod \ 11)$

   然后化简得到

   $x\equiv1(mod \ 3),-x\equiv1(mod \ 5)$

   $-2x\equiv3(mod \ 7),-3x\equiv6(mod \ 11)$

   接下来求解方程组，参照实例 1。

6. 解同余方程组

   $x\equiv3(mod \ 7),6x\equiv10(mod \ 8)$

7. 解同余方程组

   $3x\equiv1(mod \ 10),4x\equiv7(mod \ 15)$

## 背景知识

本节讨论一次同余方程组。

1. 孙子定理

   设 $m_1,\cdots,m_k$ 是两两既约的正整数，那么，对任意整数 $a_1,\cdots,a_k$ ，一次同余方程组

   $x\equiv a_j(mod \ m_j),1\le j \le k \cdots(1)$

   必有解，且解数为 1，事实上，同余方程组（1）的解是

   $x\equiv M_1M_1^{-1}a_1 + \cdots + M_kM_k^{-1}a_k(mod \ m)\cdots(2)$

   这里 $m=m_1\cdots m_k,m=m_jM_j(1\le j \le k)$ ， 以及 $M_j^{-1}$ 是满足

   $M_jM_j^{-1}\equiv 1(mod \ m_j),1\le j\le k$

   的一个整数（即是 $M_k$ 对模 $m_j$ 的逆）。

2. $x\equiv M_1M_1^{-1}x_1 + \cdots + M_kM_k^{-1}x_k\cdots(3)$

   当 $x_1,\cdots,x_k$ 分别遍历模 $m_1,\cdots$ ，模 $m_k$ 的完全（既约）剩余系时，x 遍历模 m 的完全（既约）剩余系时，x 遍历模 m 的完全（既约）剩余系，且有

   $x\equiv x_j(mod \ m_j),1\le j \le k$

3. 设 $a_1,\cdots,a_k$ 是给定整数，$(a_j,m_j)=1,1\le j\le k$ ，再设

   $x^\cdot=M_1a_1x_1+\cdots+M_ka_kx_k\cdots(4)$

   那么当 $x_1,\cdots, x_k$ 分别遍历模 $m_1,\cdots$ ，模 $m_k$ 的完全（既约）剩余系时，$x^\cdot$ 遍历模 m 的完全（既约）剩余系。

4. Euler 函数 φ(m) 时积性函数，即

   $\phi(m_1,m_2)=\phi(m_1)\phi(m_2),(m_1,m_2)=1.$

5. 设 $m_1,\cdots,m_k$ 两两互素，$m=m_1\cdots m_k$ ，以及 $f(x)$ 是整系数多项式，我们有

   $T(f;m)=T(f;m_1)\cdots T(f;m_k)$

   这里 $T(fn)$ 表示同余方程 $f(x)\equiv0(mod \ n)$ 的解数，也就是说，解数 $T(f;n)$ 是 n 的积性函数。

{{site.math}}