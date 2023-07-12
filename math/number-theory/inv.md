# 乘法逆元

若 $ax \equiv 1 \pmod{p}$，则称 $x$ 是 $a$ 在模 $p$ 意义下的乘法逆元。

> 板子题网址1: https://www.luogu.com.cn/problem/P3811
>
> 板子题网址2: https://www.luogu.com.cn/problem/P5431

## 欧拉定理求逆元

```cpp
inline int inv(int a, int p) {
    return quick_pow(a, phi[p], p);
}
```

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
// 适用于 p 为质数的情况
inline void init_inv(int n, int p) {
    inv[1] = 1;
    for (int i = 2; i <= n; ++i) {
        inv[i] = (long long)(p - p / i) * inv[p % i] % p;
    }
}
```

证明:
1. $p$ 可表示为 $p = i * k + r$，其中 $0 \leq r < i$
2. 所以 $i * k + r \equiv 0 \pmod{p}$
3. 两边同时乘以 $inv[i]$ 和 $inv[r]$，得到 $k \times inv[r] + inv[i] \equiv 0 \pmod{p}$
4. 所以 $inv[i] \equiv k \times inv[r]$ 其中 $k = \left \lfloor \frac{p}{i}  \right \rfloor$
5. 所以 $inv[i] \equiv (p - p / i) \times inv[p \pmod{i}]$

## 前缀预处理法求数组逆元

1. 设数组 `a[]` 的前缀积数组为 `f[]` , 设前缀积的逆元数组为 `g[]`
2. 预处理出 `f[]`, 并求出 `g[n] = inv(f[n])`
3. 然后根据 `g[i] = g[i + 1] * a[i + 1]` 递推出 `g[1] ~ g[n - 1]`
4. 最后得到 `inv[a[i]] = g[i] * f[i - 1]`

```cpp
inline void init_inv(int n, int p) {
    f[0] = 1;
    for (int i = 1; i <= n; ++i) {
        f[i] = (long long)f[i - 1] * a[i] % p;
    }
    g[n] = inv(f[n], p);
    for (int i = n - 1; i >= 1; --i) {
        g[i] = (long long)g[i + 1] * a[i + 1] % p;
    }
    for (int i = 1; i <= n; ++i) {
        inv[a[i]] = (long long)g[i] * f[i - 1] % p;
    }
}
```
