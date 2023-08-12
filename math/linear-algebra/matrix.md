# 矩阵运算

## 矩阵的快速幂

> 板子题网址: https://www.luogu.com.cn/problem/P3390

```cpp
struct matrix {
    LL a[N][N]{};
    LL* operator[](int x) { return a[x];}
};

matrix operator*(matrix& a, matrix& b) {
    matrix c;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            for (int k = 0; k < n; k++)
                c[i][j] = (c[i][j] + a[i][k] * b[k][j]) % P;
    return c;
}

inline matrix quick_pow(matrix& a, LL k) {
    matrix ans;
    for (int i = 0; i < n; i++) ans[i][i] = 1;
    while (k) {
        if (k & 1) ans = ans * a;
        a = a * a;
        k >>= 1;
    }
    return ans;
}
```

## 矩阵加速数列

> 板子题网址: https://www.luogu.com.cn/problem/P1939
>
> 板子题网址: https://www.luogu.com.cn/problem/P1349

这是一个稍微复杂一些的例子。

$$
\begin{gathered}
f_{1} = f_{2} = 0\\
f_{n} = 7f_{n-1}+6f_{n-2}+5n+4\times 3^n
\end{gathered}
$$

我们发现, $f_{n}$和 $f_{n-1}, f_{n-2}, n$ 有关，于是考虑构造一个矩阵描述状态。

但是发现如果矩阵仅有这三个元素

$$
\begin{bmatrix}f_n& f_{n-1}& n\end{bmatrix}
$$

是难以构造出转移方程的，因为乘方运算和 $+1$ 无法用矩阵描述。

于是考虑构造一个更大的矩阵。

$$
\begin{bmatrix}f_n& f_{n-1}& n& 3^n & 1\end{bmatrix}
$$

我们希望构造一个递推矩阵可以转移到

$$
\begin{bmatrix}
f_{n+1}& f_{n}& n+1& 3^{n+1} & 1
\end{bmatrix}
$$

转移矩阵即为

$$
\begin{bmatrix}
7 & 1 & 0 & 0 & 0\\
6 & 0 & 0 & 0 & 0\\
5 & 0 & 1 & 0 & 0\\
12 & 0 & 0 & 3 & 0\\
5 & 0 & 1 & 0 & 1
\end{bmatrix}
$$
