# 分块

## 除法分块

> 板子题网址: https://www.luogu.com.cn/problem/P2261

给出正整数 $n$ 和 $k$，请计算

$$
G(n, k) = \sum_{i = 1}^n k \bmod i
$$

思路分析:
1. 因为 $a \bmod b = a - \lfloor \frac{a}{b} \rfloor b$，所以 $G(n, k) = \sum_{i = 1}^n k - \lfloor \frac{k}{i} \rfloor i$ = $nk - \sum_{i = 1}^n \lfloor \frac{k}{i} \rfloor i$。
2. 对于 $\lfloor \frac{k}{i} \rfloor$ 我们可以进行分块。那么问题就在怎么确定左右边界。
3. 左边界 $l$ 一开始从 $i=1$ 开始，算完一块之后等于 $r+1$ 即可。
4. 右边界 $r$ 怎么算出来的呢？ 右边界 $r$ 是通过 $k/ \lfloor \frac{k}{i} \rfloor$ 来算的。**其实我们是通过了这么一个 向下取整再乘回除数的方法得到了**。

```cpp
inline void solve(){
    LL n, k;
    cin >> n >> k;
    LL ans = n * k;
    for (LL l = 1, r; l <= n; l = r + 1) {
        if (k / l != 0) r = min(k / (k / l), n);
        else r = n;
        ans -= (k / l) * (l + r) * (r - l + 1) / 2;
    }
    cout << ans << endl;
}
```
