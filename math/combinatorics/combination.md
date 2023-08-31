# 组合数

> 板子题网址: https://www.luogu.com.cn/problem/B3717

1. $C_n^m = \frac{n!}{m!(n-m)!}$
2. $C_n^m = C_{n-1}^m + C_{n-1}^{m-1}$
3. $C_n^m = \frac{n}{m} C_{n-1}^{m-1}$
4. $C_n^m = C_{n-1}^{m-1} + C_{n-2}^{m-1} + \cdots + C_{m-1}^{m-1}$

## 递推预处理

> 板子题网址: https://www.acwing.com/problem/content/887

```cpp
inline void get_comb(int n) {
    for (int i = 1; i <= n; i++) C[i][0] = C[i][i] = 1;
    for (int i = 2; i <= n; i++)
        for (int j = 1; j <= i; j++)
            C[i][j] = (C[i - 1][j - 1] + C[i - 1][j]) % P;
}
```

## 递推预处理 + 逆元

> 板子题网址: https://www.acwing.com/problem/content/888

```cpp
inline LL qpow(LL a, int k, int p) {
    LL ans = 1;
    while (k) {
        if (k & 1) ans = ans * a % p;
        a = a * a % p;
        k >>= 1;
    }
    return ans;
}

auto __ = []() {
    fac[0] = 1;
    for (int i = 1; i < N; i++) fac[i] = fac[i - 1] * i % P;
    ifac[N - 1] = qpow(fac[N - 1], P - 2, P);
    for (int i = N - 2; i >= 0; i--) ifac[i] = ifac[i + 1] * (i + 1) % P;
    return 0;
}();

inline LL C(int a, int b) {
    if (a < b) return 0;
    return fac[a] * ifac[a - b] % P * ifac[b] % P;
}
```

## Lucas 定理

> 板子题网址: https://www.luogu.com.cn/problem/P3807

$$
C_n^m \bmod p = C_{n/p}^{m/p} \cdot C_{n\%p}^{m\%p} \bmod p
$$

```cpp
inline int comb(int a, int b, int p) {
    LL ans = 1;
    for (int i = 1, j = a; i <= b; i++, j--) {
        ans = ans * j % p;
        ans = ans * qpow(i, p - 2, p) % p;
    }
    return ans;
}

inline int lucas(LL a, LL b, int p) {
    if (a < p && b < p) return comb(a, b, p);
    return comb(a % p, b % p, p) * lucas(a / p, b / p, p) % p;
}
```

## 高精度组合数

> 板子题网址: https://www.acwing.com/problem/content/890

```cpp
inline int get_p(int x, int p) {
    int ans = 0;
    while (x) ans += (x = x / p);
    return ans;
}

inline vector<int> comb(int a, int b) {
    vector<int> ans(1, 1);
    for (int i = 0; primes[i] <= a; i++) {
        int p = primes[i];
        int cnt = get_p(a, p) - get_p(b, p) - get_p(a - b, p);
        for (int j = 0; j < cnt; j++) ans = mul(ans, p);
    }
    return ans;
}
```

## 卡特兰数

以下问题属于 Catalan 数列：

1.  有 $2n$ 个人排成一行进入剧场。入场费 5 元。其中只有 $n$ 个人有一张 5 元钞票，另外 $n$ 人只有 10 元钞票，剧院无其它钞票，问有多少种方法使得只要有 10 元的人买票，售票处就有 5 元的钞票找零？
2.  一位大城市的律师在她住所以北 $n$ 个街区和以东 $n$ 个街区处工作。每天她走 $2n$ 个街区去上班。如果他从不穿越（但可以碰到）从家到办公室的对角线，那么有多少条可能的道路？
3.  在圆上选择 $2n$ 个点，将这些点成对连接起来使得所得到的 $n$ 条线段不相交的方法数？
4.  对角线不相交的情况下，将一个凸多边形区域分成三角形区域的方法数？
5.  一个栈（无穷大）的进栈序列为 $1,2,3, \cdots ,n$ 有多少个不同的出栈序列？
6.  $n$ 个结点可构造多少个不同的二叉树？
7.  $n$ 个 $+1$ 和 $n$ 个 $-1$ 构成 $2n$ 项 $a_1,a_2, \cdots ,a_{2n}$，其部分和满足 $a_1+a_2+ \cdots +a_k \geq 0(k=1,2,3, \cdots ,2n)$ 对与 $n$ 该数列为？

递推式: $H_n = C_{2n}^n - C_{2n}^{n-1}  = \frac{1}{n+1} C_{2n}^n$

> 板子题网址: https://www.luogu.com.cn/problem/P1044

```cpp
inline int catalan(int n) {
    return 1LL * comb(2 * n, n) * qpow(n + 1, P - 2, P) % P;
}
```
