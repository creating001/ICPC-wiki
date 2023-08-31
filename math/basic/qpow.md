# 快速幂与龟速乘

## 整数快速幂

> 板子题网址: https://www.luogu.com.cn/problem/P1226

```cpp
inline LL qpow(LL a, int k) {
    LL ans;
    for (ans = 1; k; k >>= 1, a = a * a % P)
        if (k & 1) ans = ans * a % P;
    return ans;
}
```

## 龟速乘

```cpp
inline LL qmul(LL a, LL b, LL p) {
    LL ans = 0;
    while (b) {
        if (b & 1) ans = (ans + a) % p;
        a = (a + a) % p;
        b >>= 1;
    }
    return ans;
}
```

```cpp
inline LL qmul(LL a, LL b, LL p) {
    return (LL)((__int128)a * b % p);
}
```
