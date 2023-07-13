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

> 板子题网站: https://www.luogu.com.cn/problem/P1939

```cpp

```
