# 快速幂

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

# 快速乘

时间复杂度 $O(\log b)$

```cpp
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
