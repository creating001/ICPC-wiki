# 中国剩余定理

## 定理代码实现

> 板子题网址: https://www.luogu.com.cn/problem/P1495

给定  $n$ 组非负整数  $a_i, b_i$ ，求解关于  $x$ 的方程组的最小非负整数解。

$$
\begin{cases} x \equiv b_1\ ({\rm mod}\ a_1) \\ x\equiv b_2\ ({\rm mod}\ a_2) \\ ... \\ x \equiv b_n\ ({\rm mod}\ a_n)\end{cases}
$$

其中 $a_i$ 两两互质。

```cpp
inline LL crt() {
    LL M = 1, ans = 0;
    for (int i = 0; i < n; i++) M *= a[i];
    for (int i = 0; i < n; i++) {
        LL Mi = M / a[i];
        LL ti = get_inv(Mi, a[i]);
        ans = (ans + Mi * ti % M * b[i] % M) % M;
    }
    return ans;
}
```

## 扩展中国剩余定理

> 板子题网址: https://www.luogu.com.cn/problem/P4777

给定  $n$ 组非负整数  $a_i, b_i$ ，求解关于  $x$ 的方程组的最小非负整数解。

$$
\begin{cases} x \equiv b_1\ ({\rm mod}\ a_1) \\ x\equiv b_2\ ({\rm mod}\ a_2) \\ ... \\ x \equiv b_n\ ({\rm mod}\ a_n)\end{cases}
$$

其中 $a_i$ 未必两两互质。

```cpp
inline LL excrt() {
    LL A = a[1], B = (b[1] % A + A) % A;
    for (int i = 2; i <= n; i++) {
        LL ai = a[i], bi = (b[i] % ai + ai) % ai;
        LL k1, k2;
        LL d = exgcd(A, ai, k1, k2);
        if ((bi - B) % d) return -1;
        LL t = ai / d;
        k1 = (LL) ((__int128) (bi - B) / d * k1 % t + t) % t;
        LL nexA = A / d * ai;
        B = (k1 * A % nexA + B) % nexA;
        A = nexA;
    }
    return B;
}
```
