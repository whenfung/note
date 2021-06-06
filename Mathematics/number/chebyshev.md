---
title: chebyshev 不等式
---

## 例子

1. 设 $m\ge 128$ 时有 
   $$
   \sum\limits_{m<p\le 2m} lnp >m(ln2)/6
   $$
   

## 背景知识

第二归纳法可以证明素数有无穷多个。以 $\pi(x)$ 表示不超过实数 x 的素数个数。

例如 $\pi(5)=3$

但是我们将给出 $\pi(x)$ 的上界与下界估计，这就是著名的 `chebyshev` 不等式。

1. 设实数 $x\ge 2$ ，我们有
   $$
   (\frac{lnx}{3})\frac{x}{lnx} < \pi(x) \le (6ln2)\frac{x}{lnx}
   $$
   及
   $$
   (\frac{1}{6ln2})nlnn<p_n<(\frac{8}{ln2})nlnn,n\ge2
   $$
   这里 $p_n$ 是第 n 个素数。



{{site.math}}