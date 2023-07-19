# 快速幂与龟速乘

## 整数快速幂

> 板子题网址: https://www.luogu.com.cn/problem/P1226

时间复杂度 $O(\log b)$

```cpp
inline int qpow(int a, int k, int p) {
    int ans = 1;
    while (k) {
        if (k & 1) ans = 1ll * ans * a % p;
        a = 1ll * a * a % p;
        k >>= 1;
    }
    return ans;
}
```

## 龟速乘

时间复杂度 $O(\log b)$

### 乘法取模

```cpp
//b 必须是正数
inline LL qmul(LL a, LL b, LL mod) {
    LL ans = 0;
    while (b) {
        if (b & 1) ans = (ans + a) % mod;
        a = (a + a) % mod;
        b >>= 1;
    }
    return ans;
}
```

### `__int128` 的使用

```cpp
inline LL qmul(LL a, LL b, LL mod) {
    return (LL)((__int128)a * b % mod);
}
```
