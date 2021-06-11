---
title: 1. 高等数学常用基础知识
---

## 1. 函数的概念

当一件事情的变化会引起另一件事情的变化时，便称这两件事情构成了一个函数关系，两个事情之间如何相互影响，就是一个对应法则。

### 1.1 函数的概念

设 x 和 y 是两个变量，D 是一个给定的数集，若对于每一个值 $x\in D$ ，按照一定的法则，有一个确定的值 y 与之对应，则称 y 为 x 的函数，记为 $y=f(x)$ ，称 x 为自变量，y 为因变量。x 的集合为定义域，y 的集合为值域。

### 1.2 反函数的概念

设函数 $y=f(x)$ 的定义域为 D，值域为 R。如果对于每一个 $y\in R$ ，必存在 $x\in D$ 使得 $y=f(x)$ 成立，则由此定义了一个新的函数 $x=\phi(y)$ 。这个函数就称为函数 $y=f(x)$ 的反函数，一般记为 $x=f^{-1}(y)$ ，它的定义域是 R，值域为 D。相对于反函数来说，原来的函数也称为直接函数。以下两点需要说明

- 反函数也是函数，也要保证单值性，例如 $y=x^2$ 没有反函数
- 若把 $x=f^{-1}(y)$ 和 $y=f(x)$ 的图形画在同一坐标系中，则它们完全重合，只有把 $y=f(x)$ 的反函数 $x=f^{-1}(y)$ 写成 $y=f^{-1}(x)$ 后，它们的图形才关于 y=x 对称，事实上这也是字母 x 与 y 互换的结果。

求证 $y=\frac 1 2 (e^x-e^{-x})$ 的反函数是 $y=\ln(x+\sqrt{x^2+1})$ 。

### 1.3 复合函数的概念

设 $y=f(u)$ 的定义域为 $D_1$ ，函数 $u=g(x)$ 在 D 上有定义，且 $g(D)\subset D_1$ ，则由

$y=f[g(x)] \ (x\in D)$

确定的函数，称为由函数 $u=g(x)$ 和函数 $y=f(u)$ 构成的复合函数，它的定义域为 D，u 称为中间变量，对于复合函数，重要的是熟练掌握分解与复合的技巧。

初等函数的复合函数还是初等函数。

### 1.4 基本初等函数

以下 5 类函数称为基本初等函数：幂函数、指数函数、对数函数、三角函数、反三角函数。

1. 幂函数

   - $y=x^\mu,\mu\in R$

   - 定义域和值域取决于 $\mu$ 的值，当 x > 0 时，$y=x^\mu$ 都有定义。

   - 常用的幂函数有

     $y=x,y=x^2,y=\sqrt x,y=x^3,y=\frac 1 x$

2. 指数函数

   - $y=a^x(a>0,a\neq 1),x \in R,y>0$
   - 当 a > 1 时，单调增加；当 0 < a < 1 时，单调减少 
   - 常用的指数函数：$y=e^x$
   - 特殊函数值：$a^0=1,e^0=1$

3. 对数函数

   - $y=log_ax(a>0,a\neq 1)$ 是 $y=a^x$ 的反函数。$x>0,y\in R$

   - a > 1 单调增加；0<a<1 单调减少

   - 常用的对数函数：$y=\ln x$

   - 特殊函数值：$\log_a1=0,\log_aa=1,\ln 1=0,\ln e =1$ 

   - 常用公式

     $x=e^{\ln x}, u^v=e^{\ln u^v=e^{v \ln u}}(x>0,u>0)$

4. 三角函数

   1. 正弦函数与余弦函数

      - 正弦函数 $y=\sin x$ ，余弦函数 $y=\cos x$ ，其中 $x \in R, y\in [-1,1]$ 。
      - 正弦是奇函数，余弦是偶函数
      - 周期均为 $2\pi$
      - 特殊函数值，有点多，不写了

   2. 正切函数与余切函数

      - 正切函数 $y=\tan x$ ，余切函数 $y=\cot x$
        $$
        \tan x = \frac{\sin x}{\cos x},cotx=\frac{\cos x}{\sin x}
        $$

      - 定义域：正切函数的定义域为 $x\neq kx+\frac \pi 2(k\in Z)$ 的一切实数 x，余切函数的定义域为 $x\neq k\pi(k\in Z)$ 的一切实数 x。

        值域：$y\in R$

      - 均为奇函数

      - 均以 $\pi$ 为最小正周期。

      - 特殊函数值就不写了，想象图

   3. 正割函数与余割函数

      - 正割函数 $y=\sec x$ ，余割函数 $y=\csc x$
        $$
        \sec x = \frac 1 {\cos x},\csc x = \frac 1 {\sin x}
        $$

      - 正割函数的定义域为 $x\neq k\pi+\frac \pi 2(k \in Z)$ 的一切实数 x ，余割函数的定义域为 $x \neq k\pi(k\in Z)$ 的一切实数 x。

        值域都为 $(-\infty,-1 ]\cup[1,+\infty)$ 

      - 正割函数为偶函数，余割函数为奇函数（在其定义域内）

      - 最小正周期为 $2\pi$ 。

5. 反三角函数

   1. 反正弦函数与反余弦函数

      - 反正弦函数 $y=\arcsin x$ ，反余弦函数 $y=\arccos x$

        $y=\arcsin x$ 是 $y=\sin x(-\frac \pi 2 \le x \le \frac \pi 2)$ 的反函数，$y=\arccos x$ 是 $y= \cos x(0\le x\le \pi)$ 的反函数。

      - 定义域：[-1,1]

        反正弦函数的值域为 $[-\frac \pi x, \frac \pi x]$ ，反余弦函数的值域为 $[0, \pi]$ 。

      - 反正弦函数单调增加，反余弦函数单调减少

      - 反正弦函数是奇函数，反余弦函数非奇非偶

      - 两个函数在定义域内有界

      - 性质：$\arcsin x + \arccos x = \frac \pi 2 (-1 \le x\le 1)$

      - 函数特殊值想象图像

   2. 反正切函数与反余切函数

      - 反正切函数 $y=\arctan x$ ，反余切函数 $y=arccot \ x$ 

        $y=\arctan x$ 是 $y=\tan x(-\frac \pi 2 < x < \frac \pi 2)$ 的反函数，$y=arccot \ x$ 是 $y = \cot x(0<x<\pi)$ 的反函数。

      - 定义域均为 R，反正切函数的值域为 $(-\frac \pi 2,\frac \pi 2)$ ，反余切函数的值域为 $(0,\pi)$ 。

      - 反正切函数单调增加，反余切函数单调减少

      - 反正切函数是奇函数，反余切函数非奇非偶

      - 两函数在定义域内有界。

      - 性质： $\arctan x + arccot \ x = \frac \pi x(-\infty < x < + \infty)$ 

      - 特殊函数值，自己想象图像

      - 极限：自己想象图像

### 1.5 函数大家庭中的几位重要人员

1. 分段函数

   在自变量的不同变化范围中，对应法则用不同式子来表示的函数称为分段函数，需要强调一句，分段函数是用几个式子来表示的一个（不是几个）函数，一般来说它不是初等函数，分段函数的典型形式如下

   - 绝对值函数
     $$
     y = \mid x\mid 
     \begin{cases}
     x,x \ge 0 \\
     -x,x < 0
     \end{cases}
     $$
     该函数在 x=0 处连续但不可导

     绝对值函数和最大值函数、最小值函数有某种 “亲密” 的关系。

     $U=max\lbrace f(x),g(x)\rbrace$

     $V=min\lbrace f(x),g(x)\rbrace$ 则

     $U+V=f(x) +g(x),U-V=\mid f(x)-g(x)\mid,UV=f(x)g(x)$

   - 符号函数
     $$
     y=sgn \ x = 
     \begin{cases}
     1, x>0, \\
     0,x=0, \\
     -1,x<0
     \end{cases}
     $$
     对于任何实数 x，有 $x=\mid x\mid sgn \ x$

   - 取整函数 y=[x]

     极限：$\lim\limits_{x\rightarrow0^+} [x]=0$ ，$\lim\limits_{x\rightarrow0^-} [x]=0$

2. 幂指函数

   形如 $u(x)^{v(x)}$ 的一类函数，通常将其转化为复合函数 $e^{v(x)\ln u(x)}$ 来处理，所以可见，幂指函数是初等函数。

## 2. 函数的四种特性

### 2.1 有界性

设 $f(x)$ 的定义域为 D，数集 $I\subset D$ ，如果存在某个正数 M，使对任一 $x\in I$ ，有 $\mid f(x) \mid \le M$ ，则称 $f(x)$ 在 $I$ 上有界；如果这样的 M 不存在，则称 $f(x)$ 在 $I$ 上无界

记忆方法：图像上下有两条横线把函数包起来了。

### 2.2 单调性

设 $f(x)$ 的定义域为 D，区间 $I\subset D$ 。如果对于区间 I 上任意两点 $x_1,x_2$ ，当 $x_1 < x_2$ 时，恒有 $f(x_1) < f(x_2)$ ，则称 $f(x)$ 在区间 $I$ 上单调增加。如果对于区间 I 上两点 $x_1,x_2$ ，当$x_1 <x_2$ 时，恒有 $f(x_1) > f(x_2)$ ，则称 f(x) 在区间 I 上单调减少

### 2.3 奇偶性

设 f(x) 的定义域 D 关于原点对称（即若 $x\in D$ ，则 $-x \in D$）。如果对于任一 $x\in D $  ，恒有 $f(-x)=f(x)$ ，则称 f(x) 为偶函数。如果对于任一 $x \in D$ ，恒有 $f(-x)=-f(x)$ ，则称 f(x) 为奇函数，我们熟知的是，偶函数的图形关于 y 轴对称，奇函数的图形关于原点对称。

### 2.4 周期性

设 f(x) 的定义域为 D，如果存在一个正数 T，使得对于任一 x∈D，有 $x\pm T \in D$ ，且 $f(x+T)=f(x)$ ，则称 f(x) 为周期函数，T 称为 f(x) 的周期。从几何图形上看，在周期函数的定义域内，相邻两个长度为 T 的区间上，函数的图形完全一样。

## 3. 常用基础知识

基础知识都是高中学的，但是后面经常用到，这里记录一下

### 3.1 数列基础

1. 等差数列

   - 首项为 $a_1$ ，公差为 $d(d\neq 0)$ 的数列 $a_1,a_1+d,a_1+2d,\cdots,a_1+(n-1)d,\cdots.$
   - 通项公式：$a_n=a_1+(n-1)d$
   - 前 n 项和 $S_n=\frac n 2[2a_1+(n-1)d]=\frac n 2 (a_1+a_n)$

2. 等比数列




{{site.math}}