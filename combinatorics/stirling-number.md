# 斯特林数

## 第一类斯特林数

**第一类斯特林数**（斯特林轮换数）$\begin{bmatrix} n \newline k \end{bmatrix}$ 表示将 $n$ 个两两不同的元素，划分为 $k$ 个非空圆排列的方案数。

$$
\begin{bmatrix}n\\ k\end{bmatrix}=\begin{bmatrix}n-1\\ k-1\end{bmatrix}+(n-1)\begin{bmatrix}n-1\\ k\end{bmatrix}
$$

> 板子题网址: https://www.luogu.com.cn/problem/P4609

```cpp
f[i][j] = f[i - 1][j - 1] * (i - 1) * f[i - 1][j];
```

## 第二类斯特林数

**第二类斯特林数**（斯特林子集数）$\begin{Bmatrix} n \newline k \end{Bmatrix}$ 表示将 $n$ 个两两不同的元素，划分为 $k$ 个非空子集的方案数。

$$
\begin{Bmatrix}n\\ k\end{Bmatrix}=\begin{Bmatrix}n-1\\ k-1\end{Bmatrix}+k\begin{Bmatrix}n-1\\ k\end{Bmatrix}
$$

```cpp
f[i][j] = f[i - 1][j - 1] + j * f[i - 1][j]
```
