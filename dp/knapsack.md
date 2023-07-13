# 背包问题

## 0-1背包问题

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
        for (int j = 1; j <= m; j++) dp[i & 1][j] = dp[i - 1 & 1][j];
        for (int j = 0; j < v[i]; j++) {
            int tt = -1, hh = 0;
            for (int k = j; k <= m; k += v[i]) {
                while (hh <= tt && (k - q[hh]) / v[i] > s[i])
                    hh++;
                while (hh <= tt && dp[i - 1 & 1][k] - dp[i - 1 & 1][q[tt]] >= (k - q[tt]) / v[i] * w[i])
                    tt--;
                q[++tt] = k;
                dp[i & 1][k] = max(dp[i & 1][k], dp[i - 1 & 1][q[hh]] + (k - q[hh]) / v[i] * w[i]);
            }
        }
    }
    return dp[n & 1][m];
}
```

## 混合背包问题

01背包、完全背包、多重背包的混合

```cpp
inline int mix_knapsack() {
    for (int i = 1; i <= n; i++) {
        if (s[i] == -1) {
            for (int j = v[i]; j <= m; j++)
                dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
        } else if (s[i] == 0) {
            for (int j = v[i]; j <= m; j++)
                dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
        } else {
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
    }
    return dp[m];
}
```

## 二维费用背包问题

有 $N$ 件物品和一个容量是 $V$ 的背包，背包能承受的最大重量是 $M$ 。每件物品只能用一次。体积是 $v_i$，重量是 $m_i$ ，价值是 $w_i$ 。输出最大价值。

```cpp
inline int two_dimension_knapsack() {
    for (int i = 1; i <= n; i++)
        for (int j = V; j >= v[i]; j--)
            for (int k = M; k >= m[i]; k--)
                dp[j][k] = max(dp[j][k], dp[j - v[i]][k - m[i]] + w[i]);
    return dp[V][M];
}
```

## 分组背包问题

有 $N$ 组物品和一个容量是 $V$ 的背包。每组物品有若干件，同一组内的物品最多只能选一件。每件物品的体积是 $v_i$，价值是 $w_i$。求解将哪些物品装入背包可使价值总和最大。

```cpp
inline int group_knapsack() {
    for (int i = 1; i <= n; i++)
        for (int j = V; j >= 0; j--)
            for (int k = 0; k < g[i].size(); k++)
                if (j >= g[i][k].first)
                    dp[j] = max(dp[j], dp[j - g[i][k].first] + g[i][k].second);
    return dp[V];
}
```

## 有依赖的背包问题

有 $N$ 件物品和一个容量是 $V$ 的背包。第 $i$ 件物品的体积是 $v_i$，价值是 $w_i$，依赖的物品编号是 $p_i$。求解将哪些物品装入背包可使价值总和最大。

```cpp
inline void dfs(int u) {
    for (int i = v[u]; i <= m; i++) dp[u][i] = w[u];
    for (int i = h[u]; ~i; i = nex[i]) {
        int to = e[i];
        dfs(to);
        for (int j = m; j >= v[u]; j--) {
            for (int k = v[u]; k <= j; k++) {
                dp[u][j] = max(dp[u][j], dp[u][k] + dp[to][j - k]);
            }
        }
    }
}
```

## 背包问题求方案数

有 $N$ 件物品和一个容量是 $V$ 的背包。每件物品只能使用一次。第 $i$ 件物品的体积是 $v_i$ ，价值是 $w_i$ 。求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。输出最优选法的方案数。

```cpp
inline int solution_number() {
    for (auto& x : num) x = 1;
    for (int i = 1; i <= n; i++) {
        for (int j = m; j >= v[i]; j--) {
            if (dp[j] < dp[j - v[i]] + w[i]) {
                dp[j] = dp[j - v[i]] + w[i];
                num[j] = num[j - v[i]];
            } else if (dp[j] == dp[j - v[i]] + w[i]) {
                num[j] += num[j - v[i]];
            }
            num[j] %= P;
        }
    }
    return num[m];
}
```

## 背包问题求具体方案

有 $N$ 件物品和一个容量是 $V$ 的背包。每件物品只能使用一次。第 $i$ 件物品的体积是 $v_i$ ，价值是 $w_i$ 。求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。输出字典序最小的最优选法。

```cpp

```
