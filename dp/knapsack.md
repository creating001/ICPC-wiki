# 背包问题

## 0-1背包问题

> 板子题网址:

有 $N$ 件物品和一个容量为 $V$ 的背包。第 $i$ 件物品的费用是 $c_i$，价值是 $w_i$。求解将哪些物品装入背包可使价值总和最大。

解法一: 二维动态规划

```cpp
// dp[i][j] 表示前 i 件物品恰好装入容量为 j 的背包所能获得的最大价值
inline int knapsack_01() {
    for (int i = 1; i <= n; i++)
        for (int j = 0; j <= m; j++) {
            dp[i][j] = dp[i - 1][j];
            if (j >= v[i])
                dp[i][j] = max(dp[i][j], dp[i - 1][j - v[i]] + w[i]);
        }
    int ans = 0;
    for (int i = 1; i <= m; i++) ans = max(ans, dp[n][i]);
    return ans;
}
```

解法二: 滚动数组优化

```cpp
inline int knapsack_01() {
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++)
            dp[i & 1][j] = dp[i - 1 & 1][j];
        for (int j = v[i]; j <= m; j++)
            dp[i & 1][j] = max(dp[i & 1][j], dp[i - 1 & 1][j - v[i]] + w[i]);
    }
    return dp[n & 1][m];
}
```

解法三: 一维动态规划

```cpp
// dp[i] 表示容量为 i 的背包所能装的最大价值
inline int knapsack_01() {
    for (int i = 1; i <= n; i++)
        for (int j = m; j >= v[i]; j--)
            dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
    return dp[m];
}
```

## 完全背包问题

有 $N$ 种物品和一个容量为 $V$ 的背包，每种物品都有无限件可用。第 $i$ 种物品的费用是 $c_i$，价值是 $w_i$。求解将哪些物品装入背包可使价值总和最大。

```cpp
inline int complete_knapsack() {
    for (int i = 1; i <= n; i++)
        for (int j = v[i]; j <= m; j++)
            dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
    return dp[m];
}
```

## 多重背包问题

有 $N$ 种物品和一个容量为 $V$ 的背包。第 $i$ 种物品最多有 $s_i$ 件可用，每件费用是 $c_i$，价值是 $w_i$。求解将哪些物品装入背包可使价值总和最大。

解法一: 多次01背包

```cpp
inline int multiple_knapsack() {
    for (int i = 1; i <= n; i++) {
        for (int k = 1; k <= s[i]; k++)
            for (int j = m; j >= v[i]; j--) {
                dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
            }
    }
    return dp[m];
}
```

解法二: 二进制优化

```cpp
inline int multiple_knapsack() {
    for (int i = 1; i <= n; i++) {
        int k = 1;
        while (s[i] >= k) {
            for (int j = m; j >= k * v[i]; j--)
                dp[j] = max(dp[j], dp[j - k * v[i]] + k * w[i]);
            s[i] -= k;
            k *= 2;
        }
        k = s[i];
        if (k)
            for (int j = m; j >= k * v[i]; j--)
                dp[j] = max(dp[j], dp[j - k * v[i]] + k * w[i]);
    }
    return dp[m];
}
```

解法三: 单调队列优化

```cpp
inline int multiple_knapsack() {
    for (int i = 1; i <= n; i++) {
        int v, w, s;
        cin >> v >> w >> s;
        for (int j = 1; j <= m; j++) dp[i & 1][j] = dp[i - 1 & 1][j];
        for (int j = 0; j < v; j++) {
            int tt = -1, hh = 0;
            for (int k = j; k <= m; k += v) {
                while (hh <= tt && (k - q[hh]) / v > s)
                    hh++;
                while (hh <= tt && dp[i - 1 & 1][k] - dp[i - 1 & 1][q[tt]] >= (k - q[tt]) / v * w)
                    tt--;
                q[++tt] = k;
                dp[i & 1][k] = max(dp[i & 1][k], dp[i - 1 & 1][q[hh]] + (k - q[hh]) / v * w);
            }
        }
    }
    return dp[n & 1][m];
}
```
