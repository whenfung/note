---
title: 2. 一维随机变量及其分布
---

## 1. 随机变量及其分布函数的概念、性质及应用

### 1.1 随机变量的概念

随机变量就是 “其值会随机而定” 的变量。设随机试验 E 的样本空间为 $\Omega=\lbrace \omega\rbrace$ ，如果对每一个 $\omega \in \Omega$ ，都有唯一的实数 X(ω) 与之对应，并且对任意实数 x，$\lbrace \omega \mid X(\omega)\le x,\omega\in\Omega\rbrace$ 是随机事件，则称定义在 Ω 上的实值单值函数 X(ω) 为随机变量，简记为随机变量 X，一般用大写字母 X，Y，Z，… 或希腊字母 δ，η，ζ，… 来表示随机变量。

### 1.2 分布函数的概念及性质

1. 概念

   设 X 是随机变量，x 是任意实数，称函数 $F(x)=P\lbrace X\le x\rbrace(x\in R)$ 为随机变量 X 的分布函数，或称 X 服从分布 F(x)，记为 X~F(x) 。

2. 性质（也是充要条件）

   1. F(x) 是 x 的单调不减函数，即对任意实数 $x_1 < x_2$ ，有 $F(x_1)\le F(x_2)$ 。
   2. F(x) 是 x 的右连续函数，即对任意 $x_0\in R$ ，有 $\lim\limits_{x \rightarrow x_0^+}F(x)=F(x_0+0)=F(x_0)$
   3. $F(-\infty)=\lim\limits_{x \rightarrow -\infty}F(x)=0,F(+\infty)=\lim\limits_{x \rightarrow +\infty}F(x)=1$

3. 分布函数的应用（求概率）

   $P\lbrace X\le a\rbrace=F(a)$

   $P\lbrace X< a\rbrace=F(a-0)$

   $P\lbrace X= a\rbrace=F(a)-F(a-0)$

## 2. 常见的两类随机变量（离散型随机变量和连续型随机变量）

### 2.1 离散型随机变量及其概率分布

如果随机变量 X 只可能取有限个或可列个值 $x_1,x_2,\cdots$ ，则称 X 为离散型随机变量，称

$p_i=P\lbrace X=x_i\rbrace,i=1,2,\cdots$

为 X 的分布列、分布律或概率分布，记为 X~p，概率分布常常用表格形式或矩阵形式表示，即

| X    | $ x_1 $ | $x_2$ | ……   |
| ---- | ------- | ----- | ---- |
| P    | $p_1$   | $p_2$ | ……   |

或 
$$
X \sim 
\begin{pmatrix} 
x_1 & x_2 & \cdots \\ 
p_1 & p_2 & \cdots 
\end{pmatrix}
$$


数列 $\lbrace p_i\rbrace$ 是离散型随机变量的概率分布的充要条件是：$p_i\ge0(i=1,2,\cdots)$ ，且 $\sum\limits_{i}p_i =1$

设离散型随机变量 X 的概率分布为 $p_i=P\lbrace X=x_i\rbrace$ ，则 X 的分布函数

$F(x)=P\lbrace X\le x \rbrace =\sum\limits_{x_i\le x}P\lbrace X =x_i\rbrace$

### 2.2 连续型随机变量及其概率密度

-∞导 x 的积分为 X 的概率密度函数，简称概率密度，记为 X~f(x) 。

充要条件为 -∞~+∞ 的积分为 1，显然概率百分百就是 1。

## 3. 常见的随机变量分布类型

### 3.1 离散型

1. 0-1 分布 B(1,p)

   1 的概率为 p，0 的概率为 1-p，称为 0-1 分布，记为 X~B(1,p)(0<p<1)

   硬币朝上的概率。

2. 二项分布 B(n, p)

   则 X=k 的概率是 C(n,k)p^k(1-p)^n-k

   n 个硬币有 k 个硬币朝上的概率

3. 泊松分布 P(λ)

   P{X=k}=λ^k/k!·e^-λ （λ>0），称 X 服从参数为 λ 的泊松分布，记为 X~P(λ) 。

   可近似二项分布。

4. 几何分布 G(p)

   (1-p)^k-1p 

   第 k 次成功的概率。

5. 超几何分布 H(n,N,M)

   C(M,k)C(N-M,n-k)/C(N,n)

   从有限N个物件（其中包含M个指定种类的物件）中抽出n个物件，成功抽出该指定种类的物件的次数（不放回）。

### 3.2 连续型

1. 均匀分布 U(a, b)

   a 到 b 都是 1/(b-a)

2. 指数分布 E(λ)

   概率密度为 λe^-λx ，x>0

   分布函数为 1-e^-λx

3. 正太分布 N(μ,σ^2)

   