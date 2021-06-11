---
title: 2. 极限与连续
---

## 1. 数列极限的概率、性质与定理

极限，从通俗、直观的意义上讲，是一个 ”无限趋近的过程“ 。

### 1.1 数列极限定义

对于任意的 $\epsilon >0$ （不论它多么小），总存在正整数 N，使得当 n>N 时，$\mid x_n -a\mid < \epsilon$ 恒成立，则称数 a 是数列 $\lbrace x_n \rbrace $ 的极限，或者称数列 $\lbrace x_n\rbrace$ 收敛于 a，记为

$\lim \limits_{n\rightarrow\infty } x_n =a$ 或 $x_n \rightarrow a(n\rightarrow\infty)$

如果不存在这样的数 a，就说数列 {$x_n $} 是发散的。

-  常用语言如下

  $\lim \limits_{n\rightarrow\infty } x_n =a$ 等价于

  $\exists N\in N_+,when \ n > N,\mid x_n-a \mid < \epsilon.$

### 1.2 数列收敛的充要条件

1. 子列

   从数列中选取无穷多项，并按原来的先后顺序组成新的数列，称新数列为原数列的子列。

2. 若数列 {$a_n$} 收敛，则其子列也收敛，且都收敛到相同的值。

### 1.3 收敛数列的性质

1. 唯一性

   如果数列极限存在，则唯一。

2. 有界性

   如果某数列收敛，该数列必有界。

3. 保号性

   如果数列收敛到 a 且 a 非零，那么从第 N 项开始，数列元素和 a 同号。

### 1.4 极限运算规则

设 $\lim\limits_{x\rightarrow\infty}x_n=a,\lim\limits_{x\rightarrow\infty}y_n=b$ ，则

1. $\lim\limits_{x\rightarrow\infty}(x_n \pm y_n)=a \pm b$
2. $\lim\limits_{x\rightarrow\infty}x_n  y_n=a b$
3. 若 $b\neq0,y_n\neq0$ ，则 $\lim\limits_{x\rightarrow\infty} \frac{x_n}{y_n}=\frac a b$

运算规则可以推广至有限个数列情形。

### 1.5 数列极限存在准则

1. 夹逼定理

   若 $y_n \le x_n \le z_n,(n=1,2,3,\cdots)$ ，且 $\lbrace y_n \rbrace$  和 $\lbrace z_n \rbrace$ 的极限都是 a，则 $\lbrace x_n \rbrace$ 的极限也是 a。

2. 单调有限数列必有极限。

例子：利用 2 证明数列 $\lbrace (1+ \frac 1 n)^n \rbrace$ 的极限存在。

## 2. 函数极限的概念、性质与定理

### 2.1 邻域

1. 一维的情形

   邻域：以点 $x_0$ 为中心的任何开区间称为点 $x_0$ 的领域，记为 $\bigcup(x_0)$ 。

   δ 领域：设 δ 是一正数，则称开区间 $(x_0-\delta,x_0 +\delta)$ 为点 $x_0$ 的 δ 邻域，记为 $\bigcup(x_0,\delta)$ ，即

   $U(x_0,\delta)=\lbrace x \mid x_0-\delta<x<x_0+\delta \rbrace = \lbrace x_0\mid\mid x-x_0\mid <\delta \rbrace$

   其中点 $x_0$ 称为邻域的中心，δ 称为邻域的半径。

   去心 δ 邻域：$\bigcup\limits^\circ(x_0,\delta)=\lbrace x_0\mid0 < \mid x-x_0\mid <\delta \rbrace$

   左、右 δ 邻域：$\lbrace x_0\mid0 <  x-x_0 <\delta \rbrace$ 称为点 $x_0$ 的右 δ 邻域，记为 $U^+(x_0,\delta)$ ；$\lbrace x_0\mid0 <  x_0-x <\delta \rbrace$ 称为点 $x_0$ 的左 δ 邻域，记为 $U^-(x_0,\delta)$ 。

2. 二维的情形

   δ 邻域：以 $ P_0$ 为原点，δ 为半径的区域

   去心 δ 邻域：不包括 $P_0$ δ 邻域。

   δ 邻域的几何意义：$U(P_0,\delta)$ 表示 $xOy$ 平面上以点 $P_0(x_0,y_0)$ 为中心，δ>0 为半径的圆的内部的点 P(x,y) 的全体。

邻域与区间（区域）：邻域当然属于区间（区域）的范畴，但事实上，邻域通常表示 “一个局部位置” ，比如  "点 $x_0$ 的 δ 邻域" ，就可以称为 “点 $x_0$ 的附近” 。于是，函数 f(x) 在点 $x_0$ 的某 δ 邻域内有定义也就是函数 f(x) 在点 $x_0$ 的附近有定义，这个 “附近” 到底有多近多远，既难以说明也没有必要说明。

例如：设函数 y=y(x) 由方程 $y\ln y-x+y=0$ 确定，试判断曲线 y=y(x)  在点 (1,1) 附近的凹凸性。

### 2.2 函数极限的定义

设函数 $f(x)$ 在点 $x_0$ 的某一去心邻域内有定义，若存在常数 A，对于任意给定的 ε>0（不论它多么小），总存在整数 δ ，使得当 $0 < \mid x-x_0\mid < \delta$ 时，对应的函数值 $f(x)$ 都满足不等式 $\mid f(x)-A\mid<\varepsilon$ ，则 A 就叫作函数 $f(x)$ 当 $x\rightarrow x_0$ 时的极限，记为

$\lim\limits_{x\rightarrow x_0}f(x)=A$ 或 $f(x)\rightarrow A (x\rightarrow x_0)$

写成 ε-δ 语言：$\forall \epsilon>0,\exists \delta>0,when \ 0<\mid x-x_0\mid<\delta,have \ \mid f(x)-A\mid<\epsilon.$

### 2.3 函数的单侧极限

若当 $x\rightarrow x_0^-$ 时，$f(x)$ 无限接近于某常数 A，则常数 A 叫作函数 $f(x)$ 当 $x\rightarrow x_0$ 时的左极限，记为

$\lim\limits_{x\rightarrow x_0^-}f(x)=A$ 或 $f(x_0^-) = A$

若当 $x\rightarrow x_0^+$ 时，$f(x)$ 无限接近于某常数 A，则常数 A 叫作函数 $f(x)$ 当 $x\rightarrow x_0$ 时的右极限，记为

$\lim\limits_{x\rightarrow x_0^+}f(x)=A$ 或 $f(x_0^+) = A$

### 2.4 函数极限存在的充要条件

$\lim\limits_{x\rightarrow x_0}f(x)=A \iff \lim\limits_{x\rightarrow x_0^-}f(x)=A \ and \ \lim\limits_{x\rightarrow x_0^+}f(x)=A$ 

$\lim\limits_{x\rightarrow x_0}f(x)=A \iff f(x)=A+\alpha(x), \lim\limits_{x\rightarrow x_0}\alpha(x)=0$

### 2.5 函数极限的性质

1. 唯一性

   如果极限 $\lim\limits_{x\rightarrow x_0}f(x)$ 存在，那么极限唯一。

2. 局部有界性

   如果 $\lim\limits_{x\rightarrow x_0}f(x)=A$ ，则存在正整数 M 和 δ，使得当 $0<\mid x-x_0\mid<\delta$ 时，有 $\mid f(x)\mid \le M$
   
3. 局部保号性

   如果 $f(x)\rightarrow A (x\rightarrow x_0)$ ，且 A>0（或 A<0），那么存在常数 δ>0 ，使得当 $0<\mid x-x_0\mid<\delta$ 时，有 f(x)>0（或 f(x)<0）。

### 2.6 无穷小与无穷大

1. 无穷小定义

   如果当 $x\rightarrow x_0$ （或 x→∞）时，函数 f(x) 的极限为零，那么称函数 f(x) 为当 $x\rightarrow x_0$ (或 x→∞) 时的无穷小，记为

   $\lim\limits_{x\rightarrow x_0}f(x)=0(or \ \lim\limits_{x\rightarrow \infty}f(x)=0).$

   特别地，以零为极限的数列 $\lbrace x_0\rbrace$ 称为 n→∞ 时的无穷小。

2. 无穷大定义

   如果当 $x\rightarrow x_0$ （或 x→∞）时，函数 $\mid f(x)\mid $ 无限增大，那么称函数 f(x) 为当 $x\rightarrow x_0$ (或 x→∞) 时的无穷大，记为

   $\lim\limits_{x\rightarrow x_0}f(x)=\infty(or \ \lim\limits_{x\rightarrow \infty}f(x)=\infty).$

   特别地，以零为极限的数列 $\lbrace x_0\rbrace$ 称为 n→∞ 时的无穷小。

3. 无穷小和无穷大的关系

   在自变量的同一变化过程中，如果 $f(x)$ 为无穷大，则 $1/f(x)$ 为无穷小；反之，如果 f(x) 为无穷小，且 $f(x) \neq 0$ ，则 $1/f(x)$ 为无穷大。

4. 无穷小的比阶

   设在同一自变量的变化过程中，$\lim \alpha(x)=0$ ，$\lim \beta(x)=0$ ，且 $\beta(x)\neq0$ ，则

   1. 若 $\lim \alpha(x)/\beta(x)=0$ ，则称 α(x) 是比 β(x) 高阶的无穷小，记为 α(x)=o(β(x))；
   2. 若 $\lim \alpha(x)/\beta(x)=\infty$ ，则称 α(x) 是比 β(x) 低阶的无穷小；
   3. 若 $\lim \alpha(x)/\beta(x)=c\neq 0$ ，则称 α(x) 与 β(x) 是同阶的无穷小；
   4. 若 $\lim \alpha(x)/\beta(x)=1$ ，则称 α(x) 与 β(x) 是等阶的无穷小，记为 α(x)~β(x)；
   5. 若 $\lim \alpha(x)/[\beta(x)]^k=c \neq 0,k>0$ ，则称 α(x) 是 β(x) k 阶的无穷小；

   注意：并不是任意两个无穷小都可进行比阶的，例如当 x→0 时，$x\sin \frac 1 x$ 与 $x^2$ 虽然都是无穷小，但不可比阶。

### 2.7 极限运算规则

若 $\lim f(x)=A,\lim g(x)=B$ ，那么

1. $\lim[kf(x)\pm lg(x)] =k\lim f(x)\pm l\lim g(x)=kA\pm lB$ ，其中 k，l 为常数；

2. $\lim[f(x)\cdot g(x)]=\lim f(x) \cdot \lim g(x)=A\cdot B$ ，特别地，若 $\lim f(x)$ 存在，n 为正整数，则

   $\lim [f(x)]^n=[\lim f(x)]^n$

3. $\lim \frac {f(x)} {g(x)}= \frac {\lim f(x)} {\lim g(x)} = \frac A B(B\neq 0)$

特别地，我们提出下面的无穷小运算规则

### 2.8 无穷小运算规则

1. 有限个无穷小的和时无穷小

2. 有界函数与无穷小的乘积是无穷小

3. 有限个无穷小的乘积是无穷小，（注意是有限个，不是无穷个）

4. 无穷小的运算

   设 m，n 为正整数，则

   1. $o(x^m)\pm o(x^n)=o(x^l),l=min\lbrace m,n\rbrace$ （加减法时低阶 “吸收” 高阶）
   2. $o(x^m)\cdot o(x^n)=o(x^{m+n}),x^m\cdot o(x^n)=o(x^{m+n})$ （乘法时阶数 “累加”）
   3. $o(x^m)= o(kx^m)=k\cdot o(x^m),k\neq 0$ 且为常数 （非零常数不影响阶数）

### 2.9 常用的等价无穷小

当 x→0 时，常用的等价无穷小有：

$\sin x\sim x,\tan x\sim x,\arcsin x\sim x$

$\arctan x \sim x,\ln(1+x)\sim x,e^x-1\sim x$

$a^x-1\sim x\ln a,1-\cos x\sim\frac 1 2 x^2,(1+x)^\alpha-1\sim\alpha x$ 

这里的 x 可以替换成一个整体，但要注意 2.6 说的注意事项

### 2.10 函数极限存在准则：夹逼准则

如果函数 f(x)，g(x) 及 h(x) 满足下列条件：

1. $g(x)\le f(x)\le h(x)$ 
2. $\lim g(x)=A,\lim h(x)=A$

则 $\lim f(x)$ 存在，且 $\lim f(x)=A$ 。

### 2.11 洛必达法则

1. 法则一。设

   1. 当 $x\rightarrow a$ （或 $x\rightarrow \infty$）时，函数 f(x) 及 F(x) 都趋于零
   2. f'(x) 及 F‘(x) 在点 a 的某去心邻域内（或当 $\mid x\mid > X$ ，此时 X 为充分大的正数）存在，且 $F'(x) \neq0$
   3. $\lim\limits_{x\rightarrow a}\frac {f'(x)}{F'(x)}$ （或 $\lim\limits_{x\rightarrow \infty}\frac {f'(x)}{F'(x)}$）存在或无穷大

   则 
   $$
   \lim\limits_{x\rightarrow a}\frac {f(x)}{F(x)} = \lim\limits_{x\rightarrow a}\frac {f'(x)}{F'(x)}
   (or \ \lim\limits_{x\rightarrow \infty}\frac {f(x)}{F(x)} = \lim\limits_{x\rightarrow \infty}\frac {f'(x)}{F'(x)})
   $$

2. 法则二。设

   1. 当 $x\rightarrow a$ （或 $x\rightarrow \infty$）时，函数 f(x) 及 F(x) 都趋于无穷大
   2. f'(x) 及 F‘(x) 在点 a 的某去心邻域内（或当 $\mid x\mid > X$ ，此时 X 为充分大的正数）存在，且 $F'(x) \neq0$
   3. $\lim\limits_{x\rightarrow a}\frac {f'(x)}{F'(x)}$ （或 $\lim\limits_{x\rightarrow \infty}\frac {f'(x)}{F'(x)}$）存在或无穷大

   则
   $$
   \lim\limits_{x\rightarrow a}\frac {f(x)}{F(x)} = \lim\limits_{x\rightarrow a}\frac {f'(x)}{F'(x)}
   (or \ \lim\limits_{x\rightarrow \infty}\frac {f(x)}{F(x)} = \lim\limits_{x\rightarrow \infty}\frac {f'(x)}{F'(x)})
   $$

其实两个法则无非就是 0/0 和 ∞/∞ 。

### 2.12 海涅定理（归结原则）

设 f(x) 在 $\bigcup\limits^\circ(x_0,\delta)$ 内有定义，则

$\lim\limits_{x\rightarrow x_0}f(x)=A$ 存在 $\iff$ 对任何以 $x_0$ 为极限的数列 $\lbrace x_n\rbrace(x_n\neq x_0)$ ，极限 $\lim\limits_{n\rightarrow \infty}f(x_n)=A$ 存在。

其实就是谁接近 $x_0$ ，谁就接近 A，真是的这么多废话。

## 3. 函数的连续与间断

### 3.1 连续的定义

所谓连续，有两种定义方法。

1.  设 f(x) 在点 $x_)$ 的某邻域内有定义，若

   $\lim\limits_{\triangle x\rightarrow 0} \triangle y=\lim\limits_{\triangle x\rightarrow 0}[f(x_0+\triangle x)-f(x_0)]=0$

   则称函数 f(x) 在点 $x_0$ 连续，点 $x_0$ 称为 f(x) 的连续点。

2. 设函数 f(x) 在点 $x_0$ 的某个邻域内有定义，且有 $\lim\limits_{x\rightarrow x_0}f(x)=f(x_0)$ ，则称函数 f(x) 在点 $x_0$ 处连续。

一般用第二个定义，第一个稍微考考证明题。

### 3.2 间断点的定义

以下设函数 f(x) 在点 $x_0$ 的某去心邻域内有定义。

1. 可去间断点

   若 $\lim\limits_{x\rightarrow x_0}f(x)=A\neq f(x_0)$ ($f(x_0)$ 甚至可以无定义)，则这类间断点称为可去间断点。

2. 跳跃间断点

   若 $\lim\limits_{x\rightarrow x_0^-}f(x)$ 与 $\lim\limits_{x\rightarrow x_0^+}f(x)$ 都存在但不相等，则这类间断点称为跳跃间断点。

   可去间断点和跳跃间断点统称为第一类间断点。

3. 无穷间断点

   若 $\lim\limits_{x\rightarrow x_0}f(x)=\infty$ ，则这类间断点称为无穷间断点，如函数 y=1/x 的点 x=0 为无穷间断点

4. 震荡间断点

   若 $\lim\limits_{x\rightarrow x_0}f(x)$ 震荡不存在，则这类间断点称为震荡间断点，如函数 y=sin(1/x) 在 x=0 处没有定义，且当 $x\rightarrow 0$ 时，函数值在 -1 和 1 这两个数之间交替震荡取值，极限不存在，故点 x=0 为函数 y=sin(1/x) 的震荡间断点

   无穷间断点和震荡间断点都属于第二类间断点，除此之外，还有不属于上述定义的第二类间断点，这里不讲。

 {{site.math}}