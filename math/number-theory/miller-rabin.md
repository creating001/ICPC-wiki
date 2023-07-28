# Miller-Rabin

> 板子题网址: https://www.luogu.com.cn/problem/U161288

```cpp
inline LL qpow(LL a, LL k, LL p) {
    LL ans = 1;
    while (k) {
        if (k & 1) ans = ans * a % p;
        a = a * a % p;
        k >>= 1;
    }
    return ans;
}

bool miller_rabin(LL p) {
    auto check = [](LL a, LL p) {
        LL d = p - 1, t = qpow(a, d, p);
        if (t != 1) return false;
        while (!(d & 1)) {
            d >>= 1;
            t = qpow(a, d, p);
            if (t == p - 1) return true;
            if (t != 1) return false;
        }
        return true;
    };
    const int test[]{2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37};
    if (p > 40) {
        for (auto x : test)
            if (!check(x, p)) return false;
        return true;
    }
    for (auto x : test)
        if (x == p) return true;
    return false;
}
```

# Pollard-Rho

> 板子题网址: https://www.luogu.com.cn/problem/P3383

```cpp

inline LL f(LL x, LL c, LL n) {
    return (x * x + c) % n;
}

LL pollard_rho(LL n) {
    if (n == 4) return 2;
    std::uniform_int_distribution<LL> Rand(3, n - 1);
    LL x = Rand(engine), y = x;
    LL c = Rand(engine);
    LL d = 1;
    x = f(x, c, n) % n;
    y = f(f(y, c, n) % n, c, n);
    for (int l = 1; x != y; l = min(128, l << 1)) {
        LL cnt = 1;
        for (int i = 0; i < l; i++) {
            LL tmp = cnt * abs(x - y) % n;
            if (!tmp) break;
            cnt = tmp;
            x = f(x, c, n) % n;
            y = f(f(y, c, n) % n, c, n);
        }
        d = __gcd(cnt, n);
        if (d != 1) return d;
    }
    return n;
}

LL max_p = 0;

inline void push(LL p) {
    if (p > max_p) max_p = p;
}

inline void dfs(LL n) {
    srand(time(nullptr));
    LL d = pollard_rho(n), nex_d;
    while (d == n) d = pollard_rho(n);
    nex_d = n / d;
    if (miller_rabin(d)) push(d);
    else dfs(d);
    if (miller_rabin(nex_d)) push(nex_d);
    else dfs(nex_d);
}
```
