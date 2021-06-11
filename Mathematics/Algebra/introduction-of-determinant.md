---
title: 1. 行列式的基本概念与计算
---

## 1. 行列式的本质定义（第一种定义）

如
$$
D_2 = \begin{bmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{bmatrix}
$$
解释：

将第一行作为一个向量 $\alpha_1 = (a_{11} , a_{12})$ ，第二行作为一个向量 $\alpha_2 = (a_{21},a_{22})$ ，那么根据我们高中学的知识可知

$S = D_2 = a_{11}a_{22} - a_{12}a_{21}$ 

其中 S 是两个向量围成的四边形面积。

当行列式的阶数为 3 时，表示的是 3 维物体的体积。

当行列式的阶数为 n 时，其结果为以这 n 个向量为邻边的 n 维图形的体积。

## 2. 行列式的性质

1. 行列互换，其值不变，即 $\mid A \mid = \mid A ^T \mid$
2. 行列式中某行（列）元素全为零，则行列式为零。
3. 行列式某行（列）元素有公因子 $k(k\neq 0)$ ，则 k 可提到行列式外面。逆过程称之为 “倍乘” 性质。
4. 行列式中某行（列）元素均是两个元素之和，则可拆成两个行列式之和
5. 行列式中两行（列）互换，行列式的值反号。
6. 行列式中的两行（列）元素相等或对应成比例，则行列式为零。
7. 行列式中某行（列）的 k 倍加到另一行（列），行列式的值不变。（3 、4 和 6 的结合推导出）

## 3. 行列式的逆序数法定义（第二种定义）

### 3.1 排列和逆序

1. 排列

   由 n 个数 $1,2,3,\cdots,n$ 组成的一个有序数组称为一个 n 级排列，如 23145 是一个 5 级排列， 41352 也是一个 5 级排列，n 级排列共有 $n!$ 个。

2. 逆序

   在一个 n 级排列 $i_1i_2\cdots i_s\cdots i_t \cdots i_n$ 中，若 $i_s > i_t$ ，且 $i_s$ 排在 $i_t$ 前面，则称这两个数构成一个逆序。

3. 逆序数

   一个排列中，逆序的总数称为改排列的逆序数，记为 $\tau(i_1i_2\cdots i_n)$ ，如 $\tau(231546) =3$ ，$\tau(621534)=8$ 。由小到大顺排的排列称为自然排序，显然，自然排序的逆序数为 0 。

4. 奇排列和偶排列

   排列的逆序数为奇数时，该排列称为奇排列；排列的逆序数为偶数时，该排列称为偶排列。

### 3.2 n 阶行列式的定义

$n(n\ge 2)$ 阶行列式

为
$$
\begin{vmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots &        & \vdots \\
a_{n1} & a_{n2} & \cdots & a_{nn} 
\end{vmatrix}
=\sum\limits_{j_1j_2\cdots j_n} (-1)^{\tau(j_1j_2\cdots j_n)}
a_{1j_1} a_{2j_2} \cdots a_{nj_n}
$$
解释：

这里 $\sum\limits_{j_1j_2\cdots j_n}$ 表示对所有 n 个列下标排列求和，故为 $n!$ 项之和。注意到行下标已经排列，而列下标是任一个 n 级排列，故每一项由取自不同行、不同列的 n 个元素的乘积组成，每项的正负号取决于 $ (-1)^{\tau(j_1j_2\cdots j_n)}$ ，当列下标为奇排列时，应附加负号；当列下标为偶排列时，应附加正号。

## 4. 行列式的展开定理（第三种定义）

阶数超过 3 阶的行列式，用前面的定义方法计算难免过于复杂，下面提出行列式的展开定理。

### 4.1 余子式

在 n 阶行列式中，去掉元素 $a_{ij}$ 所在的第 `i` 行、第 `j` 列元素，由剩下的元素按原来的位置与顺序组成的 `n-1` 阶行列式称为元素 $a_{ij}$ 的余子式，记作 $M_{ij}$ ，

即
$$
M_{ij} = 
\begin{vmatrix}
a_{11}    & \cdots & a_{1,j-1}   & a_{1,j+1}   & \cdots & a_{1n}   \\
\vdots    &        & \vdots      & \vdots      &        & \vdots   \\
a_{i-1,1} & \cdots & a_{i-1,j-1} & a_{i-1,j+1} & \cdots & a_{i-1,n}   \\
a_{i+1,1} & \cdots & a_{i+1,j-1} & a_{i+1,j+1} & \cdots & a_{i+1,n}\\
\vdots    &        & \vdots      & \vdots      &        & \vdots   \\
a_{n1}    & \cdots & a_{n,j-1}   & a_{n,j+1}   & \cdots & a_{nn}   \\
\end{vmatrix}
$$

### 4.2 代数余子式

余子式 $M_{ij}$ 乘 $(-1)^{i+j}$ 后称为 $a_{ij }$ 的代数余子式，记作 $A_{ij}$ ，即

$A_{ij}=(-1)^{i+j}M_{ij}$ 

显然也有 $M_{i+j}=(-1)^{i+j}A_{ij}$

### 4.3 行列式按某一行（列）展开的展开公式

行列式的值等于行列式的某行（列）元素分别乘其相应的代数余子式后再求和，

即
$$
\mid A \mid = 
\begin{cases}
a_{i1}A_{i1} + a_{i2}A_{i2} +\cdots + a_{in}A_{in} =
\sum\limits_{j=1}^na_{ij}A_{ij}(i=1,2,\cdots,n) \\
a_{1j}A_{1j} + a_{2j}A_{2j} +\cdots + a_{nj}A_{nj} =
\sum\limits_{i=1}^na_{ij}A_{ij}(j=1,2,\cdots,n) \\
\end{cases}
$$
但

行列式的某行（列）元素分别乘另一行（列）元素的代数余子式后再求和，结果为零，即

 $a_{i1}A_{k1} + a_{i2}A_{k2} +\cdots + a_{in}A_{kn} =0,i\neq k;$

$a_{1j}A_{1k} + a_{2j}A_{2k} +\cdots + a_{nj}A_{nk} =0,j\neq k.$

## 5. 几个重要的行列式

### 5.1 主对角线行列式（上（下）三角形行列式）

即 
$$
\begin{vmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
0      & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots &        & \vdots \\
0      & 0      & \cdots & a_{nn} 
\end{vmatrix}
= 
\begin{vmatrix}
a_{11} & 0      & \cdots & 0      \\
a_{21} & a_{22} & \cdots & 0      \\
\vdots & \vdots &        & \vdots \\
a_{n1} & a_{n2} & \cdots & a_{nn} 
\end{vmatrix}
=
\begin{vmatrix}
a_{11} & 0      & \cdots & 0      \\
0      & a_{22} & \cdots & 0      \\
\vdots & \vdots &        & \vdots \\
0      & 0      & \cdots & a_{nn} 
\end{vmatrix}
= 
\prod\limits_{i=1}^na_{ii}.
$$

### 5.2 副对角线行列式

即
$$
\begin{vmatrix}
0      & \cdots & 0         & a_{1n} \\
0      & \cdots & a_{2,n-1} & a_{2n} \\
\vdots &        & \vdots    & \vdots \\
a_{n1} & \cdots & a_{n,n-1} & a_{nn} 
\end{vmatrix}
=
\begin{vmatrix}
a_{11} & \cdots & a_{1,n-1} & a_{1n} \\
a_{21} & \cdots & a_{2,n-1} & 0      \\
\vdots &        & \vdots    & \vdots \\
a_{n1} & \cdots & 0         & 0 
\end{vmatrix}
= 
\begin{vmatrix}
0      & \cdots & 0         & a_{1n} \\
0      & \cdots & a_{2,n-1} & 0      \\
\vdots &        & \vdots    & \vdots \\
a_{n1} & \cdots & 0         & 0 
\end{vmatrix} \\
= (-1)^{n(n-1)/2}a_{1n}a_{2,n-1}\cdots a_{n1}
$$

### 5.3 拉普拉斯展开式

设 A 为 m 阶矩阵，B 为 n 阶矩阵，

则
$$
\begin{vmatrix}
A & O \\
O & B
\end{vmatrix}
= 
\begin{vmatrix}
A & C \\
O & B
\end{vmatrix}
=
\begin{vmatrix}
A & O \\
C & B
\end{vmatrix}
=\mid A \mid \mid B\mid.
$$
和
$$
\begin{vmatrix}
O & A \\
B & O
\end{vmatrix}
= 
\begin{vmatrix}
C & A \\
B & O
\end{vmatrix}
=
\begin{vmatrix}
O & A \\
B & C
\end{vmatrix}
=(-1)^{mn}\mid A \mid \mid B\mid.
$$
称

以上的 12 个行列式为 “基本形” 行列式

### 5.4 范德蒙德行列式

记
$$
\begin{vmatrix}
1         & 1         & \cdots & 1      \\
x_1       & x_2       & \cdots & x_n    \\
x_1^2     & x_2^2     & \cdots & x_n^2  \\
\vdots    & \vdots    &        & \vdots \\
x_1^{n-1} & x_2^{n-1} & \cdots & x_n^{n-1} 
\end{vmatrix}
= 
\prod\limits_{1\le i<j\le n}(x_j-x_i)
$$



{{site.math}}