# 矩阵运算

## 矩阵的快速幂

> 板子题网址: https://www.luogu.com.cn/problem/P3390

```cpp
struct matrix {
    LL a[N][N]{};
    LL* operator[](int x) {
        return a[x];
    }
};

matrix operator*(matrix& a, matrix& b) {
    matrix c;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            for (int k = 0; k < n; k++)
                c[i][j] = (c[i][j] + a[i][k] * b[k][j] % P) % P;
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

斐波那契数列（Fibonacci Sequence）大家应该都非常的熟悉了。在斐波那契数列当中，$F_1 = F_2 = 1$，$F_i = F_{i - 1} + F_{i - 2}(i \geq 3)$ 。

如果有一道题目让你求斐波那契数列第 $n$ 项的值，最简单的方法莫过于直接递推了。但是如果 $n$ 的范围达到了 $10^{18}$ 级别，递推就不行了，稳 TLE。考虑矩阵加速递推。

设 $Fib(n)$ 表示一个 $1 \times 2$ 的矩阵 $\left[ \begin{array}{ccc}F_n & F_{n-1} \end{array}\right]$ 。我们希望根据 $Fib(n-1)=\left[ \begin{array}{ccc}F_{n-1} & F_{n-2} \end{array}\right]$ 推出 $Fib(n)$ 。

试推导一个矩阵 $\text{base}$ ，使 $Fib(n-1) \times \text{base} = Fib(n)$ ，即 $\left[\begin{array}{ccc}F_{n-1} & F_{n-2}\end{array}\right] \times \text{base} = \left[ \begin{array}{ccc}F_n & F_{n-1} \end{array}\right]$ 。

怎么推呢？因为 $F_n=F_{n-1}+F_{n-2}$ ，所以 $\text{base}$ 矩阵第一列应该是 $\left[\begin{array}{ccc} 1 \\ 1 \end{array}\right]$ ，这样在进行矩阵乘法运算的时候才能令 $F_{n-1}$ 与 $F_{n-2}$ 相加，从而得出 $F_n$。同理，为了得出 $F_{n-1}$，矩阵 $\text{base}$ 的第二列应该为 $\left[\begin{array}{ccc} 1 \\ 0 \end{array}\right]$ 。

综上所述：$\text{base} = \left[\begin{array}{ccc} 1 & 1 \\ 1 & 0 \end{array}\right]$ 原式化为 $\left[\begin{array}{ccc}F_{n-1} & F_{n-2}\end{array}\right] \times \left[\begin{array}{ccc} 1 & 1 \\ 1 & 0 \end{array}\right] = \left[ \begin{array}{ccc}F_n & F_{n-1} \end{array}\right]$

这是一个稍微复杂一些的例子。

$$
\begin{gathered}
f_{1} = f_{2} = 0\\
f_{n} = 7f_{n-1}+6f_{n-2}+5n+4\times 3^n
\end{gathered}
$$

我们发现，$f_n$ 和 $f_{n-1}, f_{n-2}, n$ 有关，于是考虑构造一个矩阵描述状态。

但是发现如果矩阵仅有这三个元素 $\begin{bmatrix}f_n& f_{n-1}& n\end{bmatrix}$ 是难以构造出转移方程的，因为乘方运算和 $+1$ 无法用矩阵描述。

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
