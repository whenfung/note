---
title: 2. 行列式的综合计算与应用
---

## 1. 用行或列表示的行列式的性质

习惯将向量写成向量，但有如下变换供你使用

1. 转置向量的行列式值不变。
2. 行列式中某个向量为 0，行列式为 0。
3. 某个向量的 k 倍可以提出来。（多维空间的体积被分成相同的 k 份）。
4. 行列式里的向量加法可拆为两个行列式相加。
5. 交换两个向量的位置，行列式变号。
6. 行列式有两个向量相同，行列式为 0。
7. 某个向量的 k 倍加到另一个向量上，行列式不变。

## 2. 克拉默法则

### 2.1 克拉默法则

若 n 个方程 n 个未知量构成的非齐次线性方程组的系数行列式 `|A|≠0` （说明线性无关），则方程组有唯一解，且

```c++
xi= |Ai|/|A|
```

其中 `|Ai|` 是 `|A|` 中第 `i` 列元素（即 xi 的系数）替换成方程右边的常数项后构成的行列式。

### 2.2 推论

若包括 n 个方程 n 个未知量的齐次线性方程组的系数行列式 `|A|≠0` ，则方程组有唯一零解。

反之，若齐次线性方程组有非零解，则其系数行列式 `|A|=0` ，根据 `Ax=0` 得出。