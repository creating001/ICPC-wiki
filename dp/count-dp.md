# 计数DP

> 板子题网址: https://www.acwing.com/problem/content/902

```cpp
inline int split_num(int n) {
    dp[0] = 1;
    for (int i = 1; i <= n; i++)
        for (int j = i; j <= n; j++)
            dp[j] = (dp[j] + dp[j - i]) % P;
    return dp[n];
}
```
