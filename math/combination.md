# 组合数

$C_n^m = \frac{n!}{m!(n-m)!}$

> 板子题网址: https://www.luogu.com.cn/problem/B3717

## 组合数公式

1. $C_n^m = C_{n-1}^m + C_{n-1}^{m-1}$
2. $C_n^m = \frac{n}{m} C_{n-1}^{m-1}$
3. $C_n^m = C_{n-1}^{m-1} + C_{n-2}^{m-1} + \cdots + C_{m-1}^{m-1}$

## 递推预处理

> 板子题网址: https://www.acwing.com/problem/content/887

方法: 通过 $C_n^m = C_{n-1}^m + C_{n-1}^{m-1}$ 预处理出全部组合数的值

适用范围: 询问次数 $q$ 多 而 $n$ 和 $m$ 比较小

```cpp
inline void get_comb(int n) {
    for (int i = 1; i <= n; i++) C[i][0] = C[i][i] = 1;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j < i; j++)
            C[i][j] = (C[i - 1][j] + C[i - 1][j - 1]) % P;
}
```

## 递推预处理 + 逆元

> 板子题网址: https://www.acwing.com/problem/content/888

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

> 板子题网址: https://www.acwing.com/problem/content/889

定理内容: $C_n^m \bmod p = C_{n/p}^{m/p} \cdot C_{n\%p}^{m\%p} \bmod p$

适用范围: 询问次数 $q$ 少 而 $n$ 和 $m$ 巨大 且 模数 $p$ 较小

```cpp
inline int get_inv(int a, int p) {
    return quick_pow(a, p - 2, p);
}

inline int comb(int a, int b, int p) {
    LL ans = 1;
    for (int i = 1, j = a; i <= b; i++, j--) {
        ans = ans * j % p;
        ans = ans * get_inv(i, p) % p;
    }
    return ans;
}

inline int lucas(LL a, LL b, int p) {
    if (a < p && b < p) return comb(a, b, p);
    return comb(a % p, b % p, p) * lucas(a / p, b / p, p) % p;
}
```

## 高精度组合数

> 板子题网址: https://www.acwing.com/problem/content/890

适用范围: $n$ 和 $m$ 比较大 而且没有模数

```cpp
inline int get_p(int x, int p) {
    int ans = 0;
    while (x) {
        ans += x / p;
        x /= p;
    }
    return ans;
}

inline vector<int> comb(int a, int b) {
    vector<int> ans(1, 1);
    for (int i = 0; primes[i] <= a; i++) {
        int p = primes[i];
        int cnt = get_p(a, p) - get_p(b, p) - get_p(a - b, p);
        for (int j = 0; j < cnt; j++) {
            ans = mul(ans, p);
        }
    }
    return ans;
}
```
