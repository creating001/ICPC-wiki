# 乘法逆元

> 板子题网址: https://www.luogu.com.cn/problem/P3811
> 板子题网址: https://www.luogu.com.cn/problem/P5431

## 扩展欧几里得求逆元

```cpp
inline int inv(int a, int p) {
    int x, y;
    exgcd(a, p, x, y);
    return (x % p + p) % p;
}
```

## 递推法求 $[1,n]$ 逆元

$i$ 模 $p$ 意义下的逆元可以表示为 $inv[i] = (p - p / i) * inv[p \pmod{i}]  \pmod{p}$。

```cpp
inline void init_inv(int n) {
    inv[1] = 1;
    for (int i = 2; i <= n; ++i) {
        inv[i] = (P - P / i) * inv[P % i] % P;
    }
}
```

## 递推法求数组逆元

```cpp
inline void init_inv(int n) {
    f[0] = 1;
    for (int i = 1; i <= n; ++i) f[i] = f[i - 1] * a[i] % P;
    g[n] = inv(f[n], P);
    for (int i = n - 1; i >= 1; --i) g[i] = g[i + 1] * a[i + 1] % P;
    for (int i = 1; i <= n; ++i) inv[a[i]] = g[i] * f[i - 1] % P;
}
```
