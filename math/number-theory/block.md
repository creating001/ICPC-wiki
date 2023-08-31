# 分块

## 除法分块

> 板子题网址: https://www.luogu.com.cn/problem/P2261

```cpp
inline void solve(LL n, LL k){
    LL ans = n * k;
    for (LL l = 1, r; l <= n; l = r + 1) {
        if (k / l != 0) r = min(k / (k / l), n);
        else r = n;
        ans -= (k / l) * (l + r) * (r - l + 1) / 2;
    }
    cout << ans << endl;
}
```
