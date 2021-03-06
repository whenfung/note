---
title: 3. 矩阵的基本概念与运算
---

## 1. 矩阵的本质

假如英语系有 98 个女生，2 个男生；机械系有 95 个男生，5 个女生，利用表格表示如下

|          | 英语系 | 机械系 |
| -------- | ------ | ------ |
| 男生人数 | 2      | 95     |
| 女生人数 | 98     | 5      |

从列看，表达了同个系的男女人数；从行看，表达了同个性别的不同系的人数。

可以得出矩阵的初步认识，那就是表达系统信息。又如下面矩阵

```c++
1 2 3
6 7 9
2 4 6
```

这时，我们不必再给这个矩阵赋予具体背景了，总之，可以抽象为一个系统信息的表达。

这里提供两个重要观点，在后面的学习中慢慢参悟。

1.  矩阵也是由若干行（列）向量拼成的

   上面那个矩阵可以看作由三个行向量组成，也可以看作由三个列向量组成。

2. 矩阵不能运算，但是其若干行（列）向量之间存在着某种联系

   例如 `[1,2,3]` 和 `[2,3,4]` 这两个向量是平行的（存在线性关系），其他组向量却不平行，这种关系反映了矩阵的本质，也就是矩阵的秩，上面的矩阵的秩为 2 。

矩阵的秩的定义：最高阶非零子式的阶数称为矩阵的秩，记为 `r(A)` 。

若 n×n 矩阵的秩也为 n，则矩阵可逆。

同时也可以看出，矩阵秩的本质就是组成该矩阵的线性无关的向量的个数。

## 2. 矩阵的定义及其基本运算

### 2.1 矩阵的定义

由 m×n 个数排成 m 行 n 列的矩形表格，称为一个 m×n 矩阵。

当 m=n 时，称为 n 阶方阵。

两矩阵行列都相同，称为同型矩阵。

### 2.2 矩阵的基本运算

1. 相等。同型矩阵中对应位置的元素相等，则两矩阵相等。
2. 加法。同型矩阵相加 = 对应位置的元素相加形成的新矩阵。
3. 数乘矩阵。`kA` 的结果是 A 中对应位置的元素乘 k 得到的新矩阵。

加法运算和数乘运算统称为矩阵的线性运算，满足下列运算规律：

1. 交换律：A+B=B+A
2. 结合律：(A+B)+C=A+(B+C)
3. 分配律：k(A+B)=KA+KB，(k+l)A=kA+lA
4. 数和矩阵相乘的结合律：k(lA)=(kl)A=l(kA)

其中，A，B，C 是同型矩阵，k，l 是任意常数。

当 n 阶方阵 A 计算行列式时，记成 `|A|` 。

```c++
1, |kA|=k^n|A|
2. 一般，|A+B| ≠ |A| + |B|
3. A≠O 不能得到 |A|≠0
4. A≠B 不能得到 |A|≠B
```

- 矩阵的乘法。若 A 的行数和 B 的列数相等， A×B=C，则 

  ```c++
  c_ij = Σa_ik·b_kj
  ```

  矩阵乘法满足下列运算规律。

  1. 结合律：ABC=A(BC)

  2. 分配律：A(B+C)=AB+AC

     (A+B)C=AC+BC

  3. 数乘与矩阵乘积的结合律

     (kA)B=A(kB)=k(AB)

  注意

  ```c++
  A =  1  1  | B = 1 -1
      -1 -1  |    -1  1      显然 AB=O，BA≠AB
  ```

  1. 矩阵乘法一般情况下不满足交换律，即 AB≠BA
  2. AB=O 不能得到 A=O 或 B=O
  3. AB=AC，A≠O ⇨ A(B-C)=O 及 A≠O ，但还是不能得到 B=C

- 转置矩阵

  b_ji = a_ij，则 B 是 A 的转置矩阵。

  转置矩阵满足下列运算规律：

  1. (A^T)^T = A
  2. (kA)^T = kA^T
  3. (A+B)^T = A^T + B^T
  4. (AB)^T = B^T×A^T
  5. A 为方阵时，`|A|=|A^T|` 

- 向量的内积与正交

  内积：α 和 β 都是 n 维行向量，则 α^T·β=各分量相乘求和，记为 (α,β)

  正交：内积为 0 时称两向量正交。

  模：分量的平方和再开根号

  标准正交向量组：若列向量组中，两两之间内积为 0，自身与自身内积为 1，则该向量组为标准或单位正交向量组。

- 施密特标准正交化（又称正交规范化）过程

  先标准正交化

  ```c++
  β1=α1
  β2=α2-(α2,β1)/(β1,β1)·β1  // 其实就是斜边向量-投影向量=另一条直角边
  βn=αn-(α2,β(n-1))/(β(n-1),β(n-1))·β(n-1)-...-(α2,β1)/(β1,β1)·β1
  ```

  然后单位化，其实就是每个向量除以自己的模就行了。

- 矩阵的幂

  m 个 A 相乘称为 A 的 m 次幂。

  由于矩阵乘法不满足交换律，所以

  ```c++
  (A+B)^2 ≠ A^2+2AB+B^2
  (A-B)^2 ≠ A^2-2AB+B^2
  (A+B)(A-B) ≠ A^2-B^2
  (AB)^m ≠ A^mB^m
  ```

  其他公式有类似情况。

  若 f(x) 是多项式，则

  ```c++
  f(A) = a0E + a1A + ... + asA^s
  ```

- 方阵的行列式，设 A，B 是同阶方阵，则 `|AB|=|A||B|`

## 3. 特殊矩阵

1. 零矩阵：每个元素均为零的矩阵，记为 `O`。

2. 单位矩阵：主对角元素均为 1，其余元素全为零的 n 阶方阵，称为 n 阶单位矩阵，记成 `E`（或 `I`）。

3. 数量矩阵：数 k 和单位矩阵的乘积称为数量矩阵。

4. 对角矩阵：非主对角元素均为零的矩阵称为对角矩阵。

5. 上（下）三角矩阵：当 `i>(<)j` 时，`a_ij=0` 的矩阵称为上（下）三角矩阵。

6. 对称矩阵：`a_ij=a_ji` 的矩阵称为对称矩阵

7. 反对称矩阵：`a_ij=-a_ji` 的矩阵称为反对称矩阵

8. 正交矩阵：设 A 是 n 阶方阵，满足A^TA=E，则称 A 是正交矩阵。

   A 是正交矩阵 ⇿ A^TA=E ⇿ A^T=A^-1 ⇿ A 的行（列）向量组是标准正交向量组。（P41）

## 4. 分块矩阵

### 4.1 矩阵的分块

用几条纵线和横线把一个矩阵分成若干小块，每一小块称为原矩阵的子块，把子块看作原矩阵的一个元素，就得到了分块矩阵。

例如按列分块，得到 `B=[B1,B2,...,BN]` ；按行分块类似。

### 4.2 分块矩阵的基本运算

1. 加法：同型，且分法一致，则相应块相加即可。

2. 数乘：相应块乘 k 即可。

3. 乘法：将块当成元素，进行相应向量点积即可，和矩阵乘法一样，要保证块之间可乘、可加。

   注意：对于 3 的运算，分块相乘后，左边的仍再左边，右边的仍在右边，因为块也是小矩阵，矩阵的乘法是不符合交换律的。

4. 若 A，B 分别为 m，n 阶方阵，则分块对角矩阵的幂为对角块 A 和 B 的分别的幂。（P42）

   ```c++
   1. AB=O，则对 B 和 O 按列分块，得到 A[β1,β2,...,βn]=[0,0,...,0]，即 Ax=0 形式
   2. 对 B,C 按行分块，A 直接展开，得到 γi=ai1β1+ai2β2+...+ainβn
      即 C 的行向量是 B 的行向量的线性组合
      类似地，若 A,C 按列分块，B 展开，同意可以得到 C 的列向量是 A 的列向量的线性组合。
   ```

## 5. 矩阵的逆

### 5.1 逆矩阵的定义

1. 定义

   AB=BA=E，则称 A 为可逆矩阵，并称 B 是 A 的逆矩阵，且逆矩阵是唯一的，记作 A^-1 。

2. 伴随矩阵

   将行列式 `|A|` 的 n^2 个元素的代数余子式形成的矩阵称为 A 的伴随矩阵，记作 `A^*`

   且有 `AA^*=A^*A=|A|E` 。

3. A 可逆的充要条件是 `|A|≠0` 此时 `A^-1=1/|A|·A^*` 。 

### 5.2 逆矩阵的性质

设 A,B 是同阶可逆矩阵，则

1. `(A^-1)^-1 = A`
2. 若 k≠0，则 `(kA)^-1=1/k·A^-1` 
3. AB 也可逆，且 `(AB)^-1=B^-1A^-1`
4. A^T 也可逆，且 `(A^T)^-1=(A^-1)^T` ，此处可称为 “穿脱” 原则，即穿衣时先内后外，脱衣时先外后内
5. `|A^-1|=|A|^-1`

注意：A+B 不一定可逆，且 `(A+B)^-1 ≠ A^-1 + B^-1`

### 5.3 求逆矩阵的方法

1. 伴随矩阵法

   `A^-1=1/|A|·A^*`

2. 初等变换法

   `[A | E] → [E | A^-1]`

3. 定义法

   求一个矩阵 B，使 AB=E

4. 乘积求逆法，即若 A=BC，则

   `A^-1=(BC)^-1=C^-1B^-1`

5. 分块矩阵的逆，分为主对角和副对角两种情况。

   主对角：对角线上的 A 和 B 分别求逆即可。

   副对角：对角线上的 A 和 B 分别求逆后交换位置即可。