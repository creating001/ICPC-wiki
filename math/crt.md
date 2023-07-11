# 中国剩余定理

## 一般形式

> 板子题网址: https://www.luogu.com.cn/problem/P1495

给定  $n$ 组非负整数  $a_i, b_i$ ，求解关于  $x$ 的方程组的最小非负整数解。

$$
\begin{cases} x \equiv b_1\ ({\rm mod}\ a_1) \\ x\equiv b_2\ ({\rm mod}\ a_2) \\ ... \\ x \equiv b_n\ ({\rm mod}\ a_n)\end{cases}
$$

你可以假定 $a_i$ 两两互质。


```cpp
inline LL crt() {
    LL M = 1, ans = 0;
    for (int i = 1; i <= n; i++) M *= a[i];
    for (int i = 1; i <= n; i++) {
        LL Mi = M / a[i];
        LL ti = get_inv(Mi, a[i]);
        ans = (ans + Mi * ti % M * b[i] % M) % M;
    }
    return ans;
}
```

## 扩展中国剩余定理

> 板子题网址: https://www.luogu.com.cn/problem/P4777

```cpp

```
