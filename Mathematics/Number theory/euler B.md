---
title: Euler 函数 φ(m) (B)
---

## 作业

1. 设 (m,n)=1，证明：$m^{\phi(n)}+n^{\phi(m)}\equiv1(mod \ mn)$

   证：由背景知识 4 可知，$m^{\phi(n)}\equiv 1(mod \ n)$ ，$n^{\phi(m)}\equiv 1(mod \ m)$ ，可化为

   $m^{\phi(n)}+n^{\phi(m)}\equiv 1(mod \ n),m^{\phi(n)}+n^{\phi(m)}\equiv 1(mod \ m)$ 

   然后利用 (m,n)=1 ，小模变大模合并，得证。

2. 设 p 是素数，$a^p\equiv b^p(mod \ p)$。证明：$a^p\equiv b^p(mod \ p^2)$ 

   $a^p\equiv a (mod \ p),b^p \equiv b(mod \ p)$ ，所以 a，b 是同一个同余系的。
   
   $a=b+kp$  ，将 $a^P$ 替换成 $(b+kp)^p$ ，利用二次展开式。然后减去 $b^P$ 刚好是 $p^2$ 的倍数，得证。

## 实例

1. 设 $m=2^l(l \ge 3)$ ，a=5，求使 $a^d=1(mod \ m)$ 成立的最小正整数 $d=d_0$

2. 设 a>1，p 是奇素数，再设 q 是 $a^p-1$ 的素因数，证明：

   $q\mid a-1$ 或 $2p \mid q-1$ 至少有一个成立。

## 背景知识

1. φ(m) 是积性函数，即若 (m,n) = 1，则有 φ(mn)=φ(m)φ(n)。

2. 设 $1<m=p_1^{\alpha_1}\cdots p_r^{\alpha_r} $ ，我们有
   $$
   \phi(m)=(p_1^{\alpha_1}-p_1^{\alpha_1-1})\cdots(p_1^{\alpha_r}-p_r^{\alpha_r-1}) \\
   = m \prod\limits_{p \mid m} (1 - \frac 1 p)
   $$
   其中连乘号表示对 m 的不同素因数求积。

3. 由 1 可以推出

   $\sum\limits_{d\mid m}\phi(d)=m$ 

4. 设 (a,m)=1，则有

   $a^{\phi(m)}\equiv 1(mod \ m) \cdots (1)$

   叫做欧拉定理，特别当 p 为素数时，对任意的 a 有

   $a^p \equiv a(mod \ p)\cdots(2)$

   通常称式子（2）为 **Fermat 小定理 **。

5. 当 $m\ge 3$ 时必有 $2 \mid \phi(m)$

6. 设 (a,m)=1，那么，a 对模 m 的逆

   $a^{-1}\equiv a^{\phi(m)-1}(mod \ m)$

7. 设 (a,m)=1，那么， $a^d\equiv1 mod \ m$ 中存在最小正整数 $d_0$ 使式子成立。而且

   $a^0=1,a,\cdots,a^{d_0-1} \cdots (2)$ 多模 m 两两不同余。

   特别地，取 $d_0=\phi(m)$ 使，式子 (2) 是模 m 的一组既约剩余系。  

8. 对模 $2^l(l\ge 3)$ ，以下 $2^{l-1}$ 个数给出了它的一组既约剩余系

   $(-1)^{j_0}5^{j_1},0\le j_0<2,0\le j_1 < 2^{l-2}$
