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

