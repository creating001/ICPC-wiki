# 乘法逆元

若 $ax \equiv 1 \pmod{p}$，则称 $x$ 是 $a$ 在模 $p$ 意义下的乘法逆元。

> 板子题网址: https://www.luogu.com.cn/problem/P3811
>
> 板子题网址: https://www.luogu.com.cn/problem/P5431

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

## 线性求 $[1,n]$ 逆元

```cpp
// 适用于 p 为质数的情况
inline void init_inv(int n, int p) {
    inv[1] = 1;
    for (int i = 2; i <= n; ++i) {
        inv[i] = (long long)(p - p / i) * inv[p % i] % p;
    }
}
```
