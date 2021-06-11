---
title: 一次不定方程
---

## 作业

1. 求解 $x_1-2x_2-3x_3=7$

   $a_1=1,a_2=-2,a_3=-3$ ，所以

   $g_1=1,g_2=(1,-2)=1,g_3=(1,-3)=1$ 拆成两个式子如下

   $x_1-2x_2=y_2,y_2-3x_3=7$ 

   两个方程的特解分别是 $x_1 = 3y_2,x_2=y_2;y_2=10,x_3=1$

   引入参数 $t_1,t_2$ 得

   $x_1=3y_2-2t_1,x_2=y_2-t_1;y_2=10-3t_2,x_3=1-t_2$ 

   然后将 $x_1,x_2$ 中的 $y_2$ 消去即可。

2. 求解下面不定方程组

   $3x_1+7x_2=2,2x_1-5x_2+10x_3=8$

   解：第一个方程的特解是 $x_1=3,x_2=-1$ ，引入参数 t 得

   $x_1=3+7t,x_2=-1-3t$ ，将参数方程带入第二个式子，得到

   $29t+10x_3=-3$ ，有特解 $(-3,9)$ ，引入参数 s 得

   $t=-3+10s,x_3=10-29s$ 

   然后将 $x_1,x_2 $ 中的 t 替换成 s 即可。

3. $63x_1+110x_2=6893$ 有无正解？

   $x_1=71,x_2=22$ ，此时 $6893<63*110$ ，但仍然有正解。尾数是 3，因此 $x_1$ 的个位肯定是 1，试多几次就出来了。

## 实例

1. 求 $10x_1-7x_2 =17$ 的全部解。（利用背景知识 3）

2. 求 $18x_1 + 24x_2=9$ 的解。（利用背景知识 2）

3. 求 $907x_1 + 731x_2=2107$ 的解。

   利用辗转相除法将系数化小，知道你能看出来，然后再替代法返回。

4. 求 $117x_1 + 21x_2=38$ 的解。

   通过辗转相除法得到最后一项发现无论如何取不到整数，得出无解。

5. 求 $15x_1+10x_2+6x_3=61$ 的全部解。

   这里有三个未知数，因此利用辗转相除法得到最后需要两个参数，分别是 $x_4,x_5$ 。其中

   $x_1=1+2x_5$ 、$x_2=1+3x_4+3x_5$ 、$x_3=6-5x_4-10x_5$ 

6. 求 $15x_1+10x_2+6x_3=61$ 的解。（利用背景知识 4）

   $a_1=15,a_2=10,a_3=6$ 所以

   $g_1 = 15,g_2=(15,10)=5,g_3=(5,6)=1$ 。因此这个不定方程等价于 4 个变数，两个方程的不定方程组。

   $5y_2+6x_3=61$ 和 $15x_1+10x_2=5y_2$ 。  只要拆到左边都是二元即可。

   然后利用背景知识 3 ，拆开每个式子（给第 $i$ 个式子分配 $t_i$），最后将 $y_2$ 替换掉即可。
   
7. 求 $5x_1+3x_2=52$ 的全部正解。（利用背景知识 5）

   首先找到一组特解，列出参数方程组，然后利用公式求出。

8. 证明：$101x_1+37x_2=3189$ 有正整数解

   $c=3189<a_1a_2=101\cdot 37$ ，显然不能利用定理 5 的结论，但可以利用定理 5 的证明过程。 

9. 百钱百鸡问题就是不定方程求非负解过程

10. 求 $15x_1+10x_2+6x_3=61$ 的全部非负解

    得出参数 $t_1$ 和 $t_2$ 的方程组，利用 三个未知数都大于零的关系求出 $t_1$ 和 $t_2$ 的范围，代回去得到所有非负解。

## 背景知识

变数为整数的方程称为不定方程。二元一次不定方程是一次不定方程的基础，这里会重点讲到。

1. k 元一次不定方程如下

   $a_1x_1+\cdots + a_kx_k=c$

2. 不定方程有解的充要条件是 $(a_1,\cdots,a_k)|c$ ，它的解和不定方程
   $$
   \frac{a_1}{g}x_1+\cdots+\frac{a_k}{g}x_k = \frac c g
   $$
   的解相同，这里 $g=(a_1,\cdots,a_k)$ 。

3. 设二元一次不定方程 $a_1x_1+a_2x_2=c$ 有解，$x_{1,0} ，x_{2,0}$ 是它的一组解，那么它的所有解为
   $$
   \begin{cases}
   x_1 = x_{1,0} + \frac{a_2}{(a_1,a_2)}t,\\
   x_2 = x_{2,0} - \frac{a_1}{(a_1,a_2)}t,
   \end{cases}
   $$
   其中 $t=0, \pm1,\pm2,\cdots .$

4. 当 k 元一次不定方程有解时，它的通解由有 k-1 个参数的线性表达式给出。具体看例子6。

5. 二元一次方程重点突击：$a_1x_1+a_2x_2=c$

   显然，若 $a_1$ 和 $a_2$ 异号，则存在无数非负解，因此，我们只讨论 $a_1,a_2$ 均为正的情况

   1. 设 $a_1,a_2$ 及 c 均为正整数，$(a_1,a_2)=1$ ，那么，当 $c>a_1a_2-a_1-a_2$ 时，不定方程有非负解，解数等于$[c/(a_1,a_2)]$ 或 $[c/(a_1a_2)]+1$ ；当 $c=a_1a_2-a_1-a_2$ 时，方程无非负解。
   2. 设 $a_1,a_2$ 及 c 均为正整数，$(a_1,a_2)=1$ ，那么当 $c>a_1a_2$ 时，二元一次不定方程有正解，解数等于 $-[-c/(a_1a_2)]-1$ 或 $-[-c/(a_1a_2)] $ ；当 $c=a_1a_2$ 时，方程无正解。



{{site.math}}