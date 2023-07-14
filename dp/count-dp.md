# 计数DP

## 整数划分

> 板子题网址: https://www.acwing.com/problem/content/902

```cpp
// dp[j] 表示总和是j的方案数
// dp[i][j] = dp[i - 1][j] + dp[i][j - i]
inline int split_num(int n) {
    dp[0] = 1;
    for (int i = 1; i <= n; i++)
        for (int j = i; j <= n; j++)
            dp[j] = (dp[j] + dp[j - i]) % P;
    return dp[n];
}
```

```cpp
// dp[i][j] 表示所有总和是i, 表示成j个正整数的方案数
// dp[i][j] = dp[i - 1][j - 1] + dp[i - j][j]
inline int split_num(int n) {
    dp[0][0] = 1;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= i; j++)
            dp[i][j] = (dp[i - 1][j - 1] + dp[i - j][j]) % P;
    int ans = 0;
    for (int i = 1; i <= n; i++)
        ans = (ans + dp[n][i]) % P;
    return ans;
}
```
