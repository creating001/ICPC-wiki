# 莫比乌斯反演

## 数论分块

> 板子题网址: https://www.luogu.com.cn/problem/P2261

```cpp
inline LL solve(LL n, LL k){
    LL ans = n * k;
    for (int l = 1, r; l <= min(n, k); l = r + 1) {
        r = min(min(n, k), k / (k / l));
        ans = ans - k / l * (r + l) * (r - l + 1) / 2;
    }
    return ans;
}
```

## 莫比乌斯函数

$$
\mu(n)=
\begin{cases}
1&n=1\\
0&n\text{ 含有平方因子}\\
(-1)^k&k\text{ 为 }n\text{ 的本质不同质因子个数}\\
\end{cases}
$$

```cpp
const int init = [](int n) {
    mobius[1] = 1;
    for (int i = 2; i <= n; i++) {
        if (!vis[i]) prime[idx++] = i, mobius[i] = -1;
        for (int j = 0; prime[j] <= n / i; j++) {
            vis[prime[j] * i] = true;
            if (i % prime[j] == 0) break;
            mobius[prime[j] * i] = mobius[i] * -1;
        }
    }
    for (int i = 1; i <= n; i++) sum[i] = sum[i - 1] + mobius[i];
    return 0;
}(N - 1);
```

## [莫比乌斯反演](https://zhuanlan.zhihu.com/p/585474169)

> 板子题网址: https://www.luogu.com.cn/problem/P3327
> 板子题网址: https://www.luogu.com.cn/problem/SP5971
> 板子题网址: https://www.luogu.com.cn/problem/P1829
> 板子题网址: https://www.luogu.com.cn/problem/P3327

```cpp

```
