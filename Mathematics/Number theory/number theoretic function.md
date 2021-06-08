---
title: 数论函数
---

## 作业

1. 若 $f(n)$ 是积性函数，则 $f(m)f(n)=f((m,n))f([m,n])$

   证：设 $[m,n]=p_1^{\alpha_1}\cdots p_r^{\alpha_r}$ ，$(m,n)=p_1^{e_1}\cdots p_r^{r_r}$

   由于积性函数有 $f(n)=f(p_1^{\alpha_1})\cdots f(p_r^{\alpha_r})$ ，对 m，n 展开成算术基本定理式拆分即可。

2. 若不恒为零的积性函数 $f(n)$ 在 n=-1 有定义，则 $f(-1)=\pm1$ 

   $f(1)=f(-1)f(-1)$ 且 $f(1)=f(1)f(1)$ 解出即可。

3. 正整数 m 是完全数的充要条件是 $\sum\limits_{d \mid m}1/d=2$ 

    由实例 4 可知 $\sum\limits_{d \mid m} 1/d=\frac 1 m\sigma(m)=2$

   即 $\sigma(m)=2m$ 除去本身就是 m 了，说明 m 是完全数。

4. 整数 n 是素数的充要条件是 $\sigma(n)=n+1$

    必要性易证，充分性利用反证法。

5. 求 $\lbrace1,2,\cdots,n\rbrace$ 中与 n 互素的数之和。

    $S(n) =\sum\limits_{(d,n)=1,d<n}d = \sum\limits_{d  < n} d \cdot I(d,n) $  

## 实例

1. 设 $n=p_1^{\alpha_1}\cdots p_r^{\alpha_r}$ ，定义在 N 上的数论函数
   $$
   \omega(n)=
   \begin{cases}
   r,n>1; \\
   0,n=1.
   \end{cases}
   $$

   $$
   \Omega(n)=
   \begin{cases}
   \alpha_1 + \cdots + \alpha_r,n>1; \\
   0,n=1
   \end{cases}
   $$

   显见，$w(n)$ 是 n 的不同的素因数的个数，$\Omega(n)$ 是 n 的全部素因数（按重数计）的个数，这两个函数是否为积性？由他们可引入另外两个数论函数

   $\upsilon(n)=(-1)^{\omega(n)},n\ge 1$

   $\lambda(n)=(-1)^{\Omega(n)},n\ge 1$

   $v(n)$ 是积性函数，但不是完全积性的。$\lambda(n)$ 则是完全积性函数它称为 **Liouville 函数**。

2. 设 $d\in Z$ 。证明如下函数是完全积性的
   $$
   h(d)=
   \begin{cases}
   (-1)^{(d-1)/2}, 2\nmid d, \\
   0,2\mid d.
   \end{cases}
   $$

3. 设 $n\in N,\sigma(n)$ 表示 n 的所有正除数之和，证明：$\sigma(n)$ 是积性函数，当 $n>1$ 时
   $$
   \sigma(n)=\prod\limits_{j=1}^r \frac{p_j^{\alpha_j+1}-1}{p_j-1}
   $$
   $\sigma(n)$ 称为除数和函数。

   tips：利用 $n=p_1^{\alpha_1}\cdots p_r^{\alpha_r}$ 替代，利用组合数求和即可。

4. 设 n 是正整数，求 $\sum\limits_{d\mid n} \frac 1 d$

   $\sum\limits_{d \mid n}\frac 1 d=\sum\limits_{d \mid n}\frac 1 {(n/d)}=\frac{1}{n}\sum\limits_{d\mid n}d=\frac 1 n \sigma(n)$ 

   这里 $\sigma(n)$ 的求法看例子 3。

## 背景知识

自变量为整数，因变量为复数的数列称为**数论函数**或**算术函数** 。

1. 积性函数

   m,n 属于整数集合 D，且

   $f(mn)=f(m)f(n),(m,n)=1$ 

   则 $f(n)$ 称为时**积性函数** 。
   
2. 完全积性函数

   $f(mn)=f(m)f(n)$ ，但是不要求 $(m,n)=1$

3. 两个（完全）积性函数之积及商（分母恒不为零）都是（完全）积性函数。

4. 设 $f(n)$ 是不恒为零的数论函数，$n=p_1{\alpha^1}\cdots p_r^{\alpha_r}$ 

   $f(n)$ 是积性函数的充要条件是

   $f(1)=1$ 及 $f(n)=f(p_1^{\alpha_1})\cdots f(p_r^{\alpha_r})$

   $f(n)$ 是完全积性的充要条件是 

   $f(1)=1$ 及 $f(n)=f^{\alpha_1}(p_1)\cdots f^{\alpha_r}(p_r)$

5. 设 $f(n)$ 是不恒为零的积性函数，那么 $F(n)=\sum\limits_{d \mid n}f(d)$ 也是积性函数。

