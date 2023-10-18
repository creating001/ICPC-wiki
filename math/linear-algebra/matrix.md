# 矩阵运算

## 矩阵乘法

> 板子题网址: https://loj.ac/p/100

```cpp
struct matrix {
    LL a[N][N]{};
    inline LL* operator[](int k) { return a[k]; }
};

inline matrix operator*(matrix& x, matrix& y) {
    matrix z;
    for (int i = 0; i < n; i++)
        for (int k = 0; k < p; k++)
            for (int j = 0; j < m; j++)
                z[i][j] = (z[i][j] + x[i][k] * y[k][j]) % P;
    return z;
}
```

## 矩阵的快速幂

> 板子题网址: https://www.luogu.com.cn/problem/P3390

```cpp
inline matrix quick_pow(matrix a, int k) {
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

> 板子题网址: https://www.luogu.com.cn/problem/P1349

```cpp

```

## 行列式求值

> 板子题网址: https://www.luogu.com.cn/problem/P7112

```cpp

```

## 矩阵求逆

> 板子题网址: https://www.luogu.com.cn/problem/P4783

```cpp

```
