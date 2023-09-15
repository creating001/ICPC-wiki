# 记忆化搜索

> 板子题网址: https://www.luogu.com.cn/problem/P1434

```cpp
inline int dfs(int x, int y) {
    if (dp[x][y]) return dp[x][y];
    dp[x][y] = 1;
    for (int i = 0; i < 4; i++) {
        int nex_r = x + dx[i];
        int nex_c = y + dy[i];
        if (nex_r < 0 || nex_c < 0 || nex_r >= r || nex_c >= c)
            continue;
        if (a[nex_r][nex_c] >= a[x][y]) continue;
        dp[x][y] = max(dp[x][y], 1 + dfs(nex_r, nex_c));
    }
    return dp[x][y];
}
```
