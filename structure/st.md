# ST表

板子题网址: https://www.luogu.com.cn/problem/P3865

```cpp
inline void init() {
    for (int i = 0; i < n; i++) dp[i][0] = a[i];
    for (int j = 1; j < S; j++) {
        for (int i = 0; i + (1 << j) - 1 <= n; i++) {
            dp[i][j] = max(dp[i][j - 1], dp[i + (1 << (j - 1))][j - 1]);
        }
    }
}

inline int query(int l, int r) {
    int k = log2(r - l + 1);
    return max(dp[l][k], dp[r - (1 << k) + 1][k]);
}
```
