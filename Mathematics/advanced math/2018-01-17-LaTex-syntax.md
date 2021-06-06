---
title: LaTex syntax
---

作为一个数学专业的学生，经常要用到数学符号和公式，写博客我是用一个叫 mathjax 的 JS 第三方库来渲染公式。写博客常用公式，因此这里总结了一些常用的数学符号表示以及公式的格式。

## 数学符号

- **字母上面的上标表示方法**

| display     | syntax    | display     | syntax    | display       | syntax      | display         | syntax        |
| ----------- | --------- | ----------- | --------- | ------------- | ----------- | --------------- | ------------- |
| $\hat{a}$   | \hat{a}   | $\check{a}$ | \check{a} | $\tilde{a}$   | \tilde{a}   | $\acute{a}$     | \acute{a}     |
| $\grave{a}$ | \grave{a} | $\dot{a}$   | \dot{a}   | $\ddot{a}$    | \ddot{a}    | $\breve{a}$     | \breve{a}     |
| $\bar{a}$   | \bar{a}   | $\vec{a}$   | \vec{a}   | $\widehat{a}$ | \widehat{a} | $\widetilde{a}$ | \widetilde{a} |

- **小写希腊字母的输入方法**

| display       | syntax      | display     | syntax    | display     | syntax    | display    | syntax   |
| ------------- | ----------- | ----------- | --------- | ----------- | --------- | ---------- | -------- |
| $\alpha$      | \alpha      | $\theta$    | \theta    | $o$         | o         | $\upsilon$ | \upsilon |
| $\beta$       | \beta       | $\vartheta$ | \vartheta | $\pi$       | \pi       | $\phi$     | \phi     |
| $\gamma$      | \gamma      | $\iota$     | \iota     | $\varpi$    | \varpi    | $\varphi$  | \varphi  |
| $\delta$      | \delta      | $\kappa$    | \kappa    | $\rho$      | \rho      | $\chi$     | \chi     |
| $\epsilon$    | \epsilon    | $\lambda$   | \lambda   | $\varrho$   | \varrho   | $\psi$     | \psi     |
| $\varepsilon$ | \varepsilon | $\mu$       | \mu       | $\sigma$    | \sigma    | $\omega$   | \omega   |
| $\zeta$       | \zeta       | $\nu$       | \nu       | $\varsigma$ | \varsigma |            |          |
| $\eta$        | \eta        | $\xi$       | \xi       | $\tau$      | \tau      |            |          |

- **大写希腊字母的输入方法**

大写希腊字母的表达方式只需要把对应的小写形式的首字母大写即可

| display  | syntax | display  | syntax |
| -------- | ------ | -------- | ------ |
| $\Gamma$ | \Gamma | $\Sigma$ | \Sigma |

- **二元关系符的表达方式**

| display       | syntax      | display       | syntax      | display   | syntax  |
| ------------- | ----------- | ------------- | ----------- | --------- | ------- |
| $<$           | <           | $>$           | >           | $=$       | =       |
| $\le$         | \le         | $\ge$         | \ge         | $\equiv$  | \equiv  |
| $\ll$         | \ll         | $\gg$         | \gg         | $\doteq$  | \doteq  |
| $\prec$       | \prec       | $\succ$       | \succ       | $\sim$    | \sim    |
| $\preceq$     | \preceq     | $\succeq$     | \succeq     | $\simeq$  | \simeq  |
| $\subset$     | \subset     | $\supset$     | \supset     | $\approx$ | \approx |
| $\subseteq$   | \subseteq   | $\supseteq$   | \supseteq   | $\cong$   | \cong   |
| $\sqsubset$   | \sqsubset   | $\sqsupset$   | \sqsupset   | $\Join$   | \Join   |
| $\sqsubseteq$ | \sqsubseteq | $\sqsupseteq$ | \sqsupseteq | $\bowtie$ | \bowtie |
| $\in$         | \in         | $\ni$         | \ni         | $\propto$ | \propto |
| $\vdash$      | \vdash      | $\dashv$      | \dashv      | $\models$ | \models |
| $\mid$        | \mid        | $\parallel$   | \parallel   | $\perp$   | \perp   |
| $\smile$      | \smile      | $\frown$      | \frown      | $\asymp$  | \asymp  |
| $:$           | :           | $\notin$      | \notin      | $\ne$     | \ne     |

- **二元运算符的表达式**

| display          | syntax         | display            | syntax           | display          | syntax         |
| ---------------- | -------------- | ------------------ | ---------------- | ---------------- | -------------- |
| $+$              | +              | $-$                | -                |                  |                |
| $\pm$            | \pm            | $\mp$              | \mp              | $\triangleleft$  | \triangleleft  |
| $\cdot$          | \cdot          | $\div$             | \div             | $\triangleright$ | \triangleright |
| $\times$         | \times         | $\setminus$        | \setminus        | $\star$          | \star          |
| $\cup$           | \cup           | $\cap$             | \cap             | $\ast$           | \ast           |
| $\sqcup$         | \sqcup         | $\sqcap$           | \sqcap           | $\circ$          | \circ          |
| $\vee$           | \vee           | $\wedge$           | \wedge           | $\bullet$        | \bullet        |
| $\oplus$         | \oplus         | $\ominus$          | \ominus          | $\diamond$       | \diamond       |
| $\odot$          | \odot          | $\oslash$          | \oslash          | $\oplus$         | \oplus         |
| $\otimes$        | \otimes        | $\bigcirc$         | \bigcirc         | $\amalg$         | \amalg         |
| $\bigtriangleup$ | \bigtriangleup | $\bigtriangledown$ | \bigtriangledown | $\dagger$        | \dagger        |
| $\lhd$           | \lhd           | $\rhd$             | \rhd             | $\ddagger$       | \ddagger       |
| $\unlhd$         | \unlhd         | $\unrhd$           | \unrhd           | $\wr$            | \wr            |

- **大尺寸运算符的表达方式**

| display   | syntax  | display     | syntax    | display     | syntax    | display      | syntax     |
| --------- | ------- | ----------- | --------- | ----------- | --------- | ------------ | ---------- |
| $\sum$    | \sum    | $\bigcup$   | \bigcup   | $\bigvee$   | \bigvee   | $\bigoplus$  | \bigoplus  |
| $\prod$   | \prod   | $\bigcap$   | \bigcap   | $\bigwedge$ | \bigwedge | $\bigotimes$ | \bigotimes |
| $\coprod$ | \coprod | $\bigsqcup$ | \bigsqcup |             |           | $\bigodot$   | \bigodot   |
| $\int$    | \int    | $\oint$     | \oint     |             |           | $\biguplus$  | \biguplus  |

- **定界符的表达方式**

| display   | syntax  | display      | syntax     | display        | syntax       | display        | syntax       |
| --------- | ------- | ------------ | ---------- | -------------- | ------------ | -------------- | ------------ |
| $（$      | （      | $）$         | ）         | $\uparrow$     | \uparrow     | $\Uparrow$     | \Uparrow     |
| $\lbrack$ | \lbrack | $\rbrack$    | \rbrack    | $\downarrow$   | \downarrow   | $\Downarrow$   | \Downarrow   |
| $\lbrace$ | \lbrace | $\rbrace$    | \rbrace    | $\updownarrow$ | \updownarrow | $\Updownarrow$ | \Updownarrow |
| $\langle$ | \langle | $\rangle$    | \rangle    | $\vert$        | \vert        | $\Vert$        | \Vert        |
| $\lfloor$ | \lfloor | $\rfloor$    | \rfloor    | $\lceil$       | \lceil       | $\rceil$       | \rceil       |
| $/$       | /       | $\backslash$ | \backslash |                |              |                |              |

- 其他运算符

| display               | syntax    | display       | syntax      |
| --------------------- | --------- | ------------- | ----------- |
| $\infty$              | \infty    | $\sqrt[n]{x}$ | \sqrt[n]{x} |
| $\iff$                | \iff      | $\forall$     | \forall     |
| $\overline{A \cap B}$ | \overline | $\exists$     | \exists     |
| $\cdots$              | \cdots    | $\vdots$      | \vdots      |



## 常见公式的格式

- **基本类型**

| display          | syntax         |
| ---------------- | -------------- |
| $\frac{a}{b}$    | \frac{a}{b}    |
| $a^x$            | a^x            |
| $a_n$            | a_n            |
| $\int_{x_0}^{x}$ | \int_{x_0}^{x} |
| $a \quad b$      | a \quad b      |
| $a \ b$          | a \ b          |



- **数组**

$$
\left(                 %左括号
  \begin{array}{ccc}   %该矩阵一共3列，每一列都居中放置
    a11 & a12 & a13\\  %第一行元素
    a21 & a22 & a23\\  %第二行元素
  \end{array}
\right)                 %右括号
$$

``` latex
\left(                 %左括号
  \begin{array}{ccc}   %该矩阵一共3列，每一列都居中放置
    a11 & a12 & a13\\  %第一行元素
    a21 & a22 & a23\\  %第二行元素
  \end{array}
\right)                 %右括号
```



- **矩阵**

matrix、pmatrix 或 bmatrix 环境：


$$
\begin{matrix} 0 & 1 \\ 1 & 0 \end{matrix}
\quad
\begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}
\quad
\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}
$$




``` latex
\begin{matrix} 0 & 1 \\ 1 & 0 \end{matrix}
\quad
\begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}
\quad
\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}
```



Bmatrix、vmatrix 或 Vmatrix 环境：


$$
\begin{Bmatrix} 1 & 0 \\ 0 & -1 \end{Bmatrix}
\quad
\begin{vmatrix} a & b \\ c & d \end{vmatrix}
\quad
\begin{Vmatrix} i & 0 \\ 0 & -i \end{Vmatrix}
$$



``` latex
\begin{Bmatrix} 1 & 0 \\ 0 & -1 \end{Bmatrix}
\quad
\begin{vmatrix} a & b \\ c & d \end{vmatrix}
\quad
\begin{Vmatrix} i & 0 \\ 0 & -i \end{Vmatrix}
```




- **方程组**

$$
f(x) =
\begin{cases}
  f_1(x) = a + b -3c   \\
  f_2(x) = 2a + 5b +c  \\
  f_3(x) = 4a + 2b -c  
\end{cases}
$$



```latex
f(x) =
\begin{cases}
  f_1(x) = a + b -3c   \\
  f_2(x) = 2a + 5b +c  \\
  f_3(x) = 4a + 2b -c  
\end{cases}
```





## 微积分

1. 极限

   $\lim\limits_{x \rightarrow x_0}f(x)$

   ```c
   \lim\limits_{x \rightarrow x_0}f(x)
   ```

2. 偏导

   $\partial$

## 概率论

1. 概率分布

   $p_i=P\lbrace X=x_i\rbrace,i=1,2,\cdots$

   

## 线性代数



{{ site.math }}