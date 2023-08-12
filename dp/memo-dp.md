# 记忆化搜索

> 板子题网址: https://www.luogu.com.cn/problem/P1434

```cpp
inline int dp(int x, int y) {
    if (f[x][y]) return f[x][y];
    f[x][y] = 1;
    for (int i = 0; i < 4; i++) {
        int nex_r = x + dx[i];
        int nex_c = y + dy[i];
        if (nex_r < 0 || nex_c < 0 || nex_r >= r || nex_c >= c)
            continue;
        if (a[nex_r][nex_c] >= a[x][y]) continue;
        f[x][y] = max(f[x][y], 1 + dp(nex_r, nex_c));
    }
    return f[x][y];
}
```
