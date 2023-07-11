# 组合数

$C_n^m = \frac{n!}{m!(n-m)!}$

## 组合数公式

1. $C_n^m = C_{n-1}^m + C_{n-1}^{m-1}$
2. $C_n^m = \frac{n}{m} C_{n-1}^{m-1}$
3. $C_n^m = C_{n-1}^{m-1} + C_{n-2}^{m-1} + \cdots + C_{m-1}^{m-1}$

## 递推预处理

方法: 通过 $C_n^m = C_{n-1}^m + C_{n-1}^{m-1}$ 预处理出全部组合数的值

适用范围: 询问次数 $q$ 多 而 $n$ 和 $m$ 比较小

```cpp
inline void get_combinations(int n) {
    assert(n < N);
    for (int i = 1; i <= n; i++) C[i][0] = C[i][i] = 1;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j < i; j++)
            C[i][j] = (C[i - 1][j] + C[i - 1][j - 1]) % P;
}
```

## 递推预处理 + 逆元

方法: 通过预处理出 阶乘 以及 阶乘的逆元 来计算组合数

适用范围: 询问次数 $q$ 多 而 $n$ 和 $m$ 比较大

```cpp
inline void init_fact(int n) {
    fact[0] = 1;
    for (int i = 1; i <= n; i++)
        fact[i] = fact[i - 1] * i % P;
    inv_fact[n] = get_inv(fact[n], P);
    for (int i = n - 1; i >= 0; i--)
        inv_fact[i] = inv_fact[i + 1] * (i + 1) % P;
}
```

## Lucas 定理

定理内容: $C_n^m \bmod p = C_{n/p}^{m/p} \cdot C_{n\%p}^{m\%p} \bmod p$

适用范围: 询问次数 $q$ 少 而 $n$ 和 $m$ 巨大 且 模数 $p$ 较小

```cpp
inline int get_inv(int a, int p) {
    return quick_pow(a, p - 2, p);
}

inline int Comb(int a, int b, int p) {
    LL ans = 1;
    for (int i = 1, j = a; i <= b; i++, j--) {
        ans = ans * j % p;
        ans = ans * get_inv(i, p) % p;
    }
    return ans;
}

inline int lucas(LL a, LL b, int p) {
    if (a < p && b < p) return Comb(a, b, p);
    return Comb(a % p, b % p, p) * lucas(a / p, b / p, p) % p;
}
```
