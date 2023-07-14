# 记忆化搜索

记忆化搜索是一种通过记录已经遍历过的状态的信息，从而避免对同一状态重复遍历的搜索实现方式。

因为记忆化搜索确保了每个状态只访问一次，它也是一种常见的动态规划实现方式。

## 记忆化搜索实现DP

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
