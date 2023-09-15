# 线性DP

## 数字三角形模型

> 板子题网址: https://www.acwing.com/problem/content/277

```cpp
inline int number_triangle(int n, int m) {
    for (int k = 2; k <= n + m; k++)
        for (int r1 = 1; r1 <= n; r1++)
            for (int r2 = 1; r2 <= n; r2++) {
                int c1 = k - r1, c2 = k - r2;
                if (c1 < 1 || c1 > m || c2 < 1 || c2 > m)
                    continue;
                if (k != n + m && r1 == r2) continue;
                int& x = dp[k][r1][r2];
                int t = g[r1][c1] + g[r2][c2];
                x = max(x, dp[k - 1][r1 - 1][r2 - 1] + t);
                x = max(x, dp[k - 1][r1 - 1][r2] + t);
                x = max(x, dp[k - 1][r1][r2 - 1] + t);
                x = max(x, dp[k - 1][r1][r2] + t);
            }
    return dp[n + m][n][n];
}
```

## 最长上升子序列

> 板子题网址: https://www.acwing.com/problem/content/898

```cpp
inline int LIS(int* a, int n) {
    for (int i = 0; i < n; i++) dp[i] = 1;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < i; j++)
            if (a[i] > a[j]) dp[i] = max(dp[i], dp[j] + 1);
    return *max_element(dp, dp + n);
}
```

```cpp
inline int LIS(int* a, int n) {
    for (int i = 0; i < n; i++) {
        if (!pos || dp[pos] < a[i]) dp[++pos] = a[i];
        else {
            int p = lower_bound(dp + 1, dp + pos + 1, a[i]) - dp;
            dp[p] = a[i];
        }
    }
    return pos;
}
```

## 最长公共子序列

> 板子题网址: https://www.luogu.com.cn/problem/P1439

```cpp
inline int LCS(int* a, int* b, int n, int m) {
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            if (a[i] == b[j])
                dp[i][j] = dp[i - 1][j - 1] + 1;
            else
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
    return dp[n][m];
}
```

## 编辑距离

> 板子题网址: https://www.luogu.com.cn/problem/P2758

```cpp
inline int edit_distance(char* a, char* b) {
    int n = strlen(a + 1), m = strlen(b + 1);
    for (int i = 0; i <= max(n, m); i++) dp[i][0] = dp[0][i] = i;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            dp[i][j] = min(dp[i - 1][j - 1] + (a[i] != b[j]),
                        min(dp[i - 1][j], dp[i][j - 1]) + 1);
    return dp[n][m];
}
```
