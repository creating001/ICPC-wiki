# BSGS

## BSGS

$$
a^{kx} \equiv b \cdot a^{y} \pmod{p}
$$

> 板子题网址: https://www.luogu.com.cn/problem/P3846

```cpp
inline int bsgs(int a, int b, int p) {
    if (1 % p == b % p) return 0;
    unordered_map<int, int> mp;
    int k = sqrt(p) + 1;
    for (int i = 0, num = b % p; i <= k - 1; i++) {
        mp[num] = i;
        num = num * a % p;
    }
    int a_k = 1;
    for (int i = 1; i <= k; i++) a_k = a_k * a % p;
    for (int i = 1, num = a_k % p; i <= k; i++) {
        if (mp.count(num)) return k * i - mp[num];
        num = num * a_k % p;
    }
    return -1;
}
```

## EXBSGS

> 板子题网址: https://www.luogu.com.cn/problem/P4195

```cpp
inline int exbsgs(int a, int b, int p) {
    b = (b % p + p) % p;
    if (1 % p == b % p) return 0;
    int x, y, d = exgcd(a, p, x, y);
    if (d != 1) {
        if (b % d) return -INF;
        exgcd(a / d, p / d, x, y);
        return exbsgs(a, b / d * x, p / d) + 1;
    }
    return bsgs(a, b, p);
}
```
