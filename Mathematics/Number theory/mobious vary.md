---
title: mobius 变换及其反转公式
---

## 作业

1. 证明：$f(n)$ 的 $M\ddot obius$ 变换的 $M\ddot obius$ 变换为 $\sum\limits_{d \mid n}f(d)\tau(n/d)$

   证：题目大致意思是进行两次 mobius 变换。那么第一次 mobius 变换是 $F_1(n)=\sum\limits_{d\mid n}f(d)$ 。

   第二次 mobius 变换就是 $F_2(m) = \sum\limits_{n\mid m}F_1(n)=\sum\limits_{n\mid m}\sum\limits_{d \mid n}f(d)$ ， 交换求和号得到

   $F_2(m)=\sum\limits_{d\mid m}(\sum\limits_{n\mid m}f(d))$ ，由于 $n\ge d$ ，括号内可化为 $f(d)\tau(m/d)$ ，得证。

2. 求 $\mid \mu(n) \mid$ 的 mobius 变换及 mobius 逆变换

   

   变换： $\sum\limits_{d\mid n}\mid\mu(n)\mid = 2^{\omega(n)}$ ， $\omega(n)$ 表示 n 的不同的素因数个数
   
    逆变换：首先，$\mu(n)$ 是积性的，即 $\mu(mn)=\mu(n)\mu(n)$ 。然后利用定义分类讨论。
   $$
   \sum\limits_{d\mid n}\mu(d)\mid \mu(n/d)\mid =
   \begin{cases}
   1, n=1,\\
   (-1)^r,n=p_1^2\cdots p_r^2, \\
   0, other
   \end{cases}
   $$

## 实例

1. 求 $Liouville$ 函数 $\lambda (n)$ 的 $M\ddot obius$ 变换。
2. 求 $\mu^2(n)/\phi(n)$ 的 $M\ddot o bius$ 变换。
3. 求 $\Omega(n)$ 的 $M\ddot o bius$ 变换。
4. 求 $F(n)=n^t$ 的 $M\ddot obius$ 逆变换 $f(n)$ 。
5. 求 $F(n)=\phi(n)$ 的 $M\ddot obius$ 变换。
6. 求 $F(n)=lnn$ 的 $M\ddot o bius$ 逆变换 $\Lambda(n)$ 。

## 背景知识

由给定的数论函数 $f(n)(n\in N)$ ，构造一个新的数论函数

$F(n)=\sum \limits_{d\mid n} f(d),n\in N  \cdots(1)$ ，例如

| $f(n)$ | 1         | n           | $\mu(n)$ | $\phi(n)$ |
| ------ | --------- | ----------- | -------- | --------- |
| $F(n)$ | $\tau(n)$ | $\sigma(n)$ | $[1/n]$  | $n$       |

当 $f(n)$ 是积性函数时，求和式是容易计算的，我们将 $F(n)$ 称为是 $f(n)$ 的 $M \ddot o bius$ 变换。而把 $f(n)$ 称为是 $F(n)$ 的 $M\ddot obius$ 逆变换。

1. 设 $f(n)$ 和 $F(n)$ 是定义在 N 上的数论函数，那么式子（1）成立的充要条件是

   $f(n)=\sum\limits_{d \mid n}\mu(d)F(\frac n d)$

2. 设 $h(n),g(n)$ 是定义在 N 上的积性函数，那么

   $H(n) = \sum\limits_{d \mid m}h(d)g(\frac n d),n\in N$ 

   这叫 $Dirichlet$ 卷积。

3. 设 $F(n)$ 是 $f(n)$ 的 $M\ddot o bius$ 变换，那么，$f(n)$ 是积性函数的充要条件是 $F(n)$ 也是积性函数。


