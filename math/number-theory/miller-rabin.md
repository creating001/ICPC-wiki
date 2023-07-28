# Miller-Rabin

> 板子题网址: https://www.luogu.com.cn/problem/U161288

```cpp
LL qpow(LL a, LL k, LL p) {
    LL ans = 1;
    while (k) {
        if (k & 1) ans = (LLL) a * ans % p;
        a = (LLL) a * a % p;
        k >>= 1;
    }
    return ans;
}

inline bool miller_rabin(LL p) {
    auto check = [](LL a, LL p) {
        LL k = p - 1, t = qpow(a, k, p);
        if (t != 1) return false;
        while (!(k & 1)) {
            k >>= 1;
            t = qpow(a, k, p);
            if (t == p - 1) return true;
            if (t != 1) return false;
        }
        return true;
    };

    const int table[]{2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37};

    if (p > 40) {
        for (auto x : table) if (!check(x, p)) return false;
        return true;
    }
    for (auto x : table) if (x == p) return true;
    return false;
}
```

# Pollard-Rho

> 板子题网址: https://www.luogu.com.cn/problem/P4718

```cpp
static mt19937_64 engine;

inline LL f(LL x, LL c, LL p) {
    return ((LLL) x * x + c) % p;
}

inline LL pollard_rho(LL n) {
    if (n == 4) return 2;
    uniform_int_distribution<LL> Rand(3, n - 1);
    LL x = Rand(engine), y = x;
    LL c = Rand(engine);
    LL d = 1;

    x = f(x, c, n);
    y = f(f(y, c, n), c, n);
    for (int l = 1; x != y; l = min(128, l << 1)) {
        LL cnt = 1;
        for (int i = 0; i < l; i++) {
            LL tmp = cnt * abs(x - y) % n;
            if (!tmp) break;
            cnt = tmp;
            x = f(x, c, n);
            y = f(f(y, c, n), c, n);
        }
        d = __gcd(cnt, n);
        if (d != 1) return d;
    }
    return n;
}

inline LL dfs(LL n) {
    LL max_p = 0;
    LL d = pollard_rho(n), nex_d;
    while (d == n) d = pollard_rho(n);
    nex_d = n / d;
    if (miller_rabin(d)) max_p = max(max_p, d);
    else max_p = max(max_p, dfs(d));
    if (miller_rabin(nex_d)) max_p = max(max_p, nex_d);
    else max_p = max(max_p, dfs(nex_d));
    return max_p;
}
```
