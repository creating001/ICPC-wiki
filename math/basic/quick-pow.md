# 快速幂与龟速乘

## 整数快速幂

> 板子题网址: https://www.luogu.com.cn/problem/P1226

时间复杂度 $O(\log b)$

```cpp
inline LL quick_pow(LL a, LL b, LL mod) {
    LL ans = 1;
    while (b) {
        if (b & 1) ans = ans * a % mod;
        a = a * a % mod;
        b >>= 1;
    }
    return ans;
}
```

## 龟速乘

时间复杂度 $O(\log b)$

### 乘法取模

```cpp
//b 必须是正数
inline LL quick_mul(LL a, LL b, LL mod) {
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
inline LL quick_mul(LL a, LL b, LL mod) {
    return (LL)((__int128)a * b % mod);
}
```
