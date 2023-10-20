# 背包问题

## 0-1背包问题

```cpp
inline int knapsack_01() {
    for (int i = 1; i <= n; i++)
        for (int j = m; j >= v[i]; j--)
            dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
    return dp[m];
}
```

## 完全背包问题

```cpp
inline int complete_knapsack() {
    for (int i = 1; i <= n; i++)
        for (int j = v[i]; j <= m; j++)
            dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
    return dp[m];
}
```

## 多重背包问题

```cpp
inline int multiple_knapsack() {
    for (int i = 1; i <= n; i++)
        for (int k = 1; k <= s[i]; k++)
            for (int j = m; j >= v[i]; j--)
                dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
    return dp[m];
}
```

二进制优化

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

单调队列优化

```cpp
inline int multiple_knapsack() {
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) dp[i & 1][j] = dp[i - 1 & 1][j];
        for (int j = 0; j < v[i]; j++) {
            int tt = -1, hh = 0;
            for (int k = j; k <= m; k += v[i]) {
                while (hh <= tt && (k - q[hh]) / v[i] > s[i]) hh++;
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
                s[i] -= k, k *= 2;
            }
            k = s[i];
            if (k) for (int j = m; j >= k * v[i]; j--)
                    dp[j] = max(dp[j], dp[j - k * v[i]] + k * w[i]);
        }
    }
    return dp[m];
}
```

## 二维费用背包问题

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

```cpp
inline int group_knapsack() {
    for (int i = 1; i <= n; i++)
        for (int j = V; j >= 0; j--)
            for (int k = 0; k < g[i].size(); k++)
                if (j >= g[i][k].F)
                    dp[j] = max(dp[j], dp[j - g[i][k].F] + g[i][k].S);
    return dp[V];
}
```

## 有依赖的背包问题

```cpp
inline void dfs(int u) {
    for (int i = v[u]; i <= m; i++) dp[u][i] = w[u];
    for (int i = h[u]; ~i; i = nex[i]) {
        int to = e[i];
        dfs(to);
        for (int j = m; j >= v[u]; j--)
            for (int k = v[u]; k <= j; k++)
                dp[u][j] = max(dp[u][j], dp[u][k] + dp[to][j - k]);
    }
}
```

## 背包问题求方案数

```cpp
inline int solution_number() {
    for (auto& x : num) x = 1;
    for (int i = 1; i <= n; i++)
        for (int j = m; j >= v[i]; j--) {
            if (dp[j] < dp[j - v[i]] + w[i]) {
                dp[j] = dp[j - v[i]] + w[i];
                num[j] = num[j - v[i]];
            } else if (dp[j] == dp[j - v[i]] + w[i]) {
                num[j] += num[j - v[i]];
            }
            num[j] %= P;
        }
    return num[m];
}
```

## 背包问题求具体方案

```cpp
inline vector<int> solution() {
    for (int i = n; i >= 1; i--)
        for (int j = 0; j <= m; j++) {
            dp[i][j] = dp[i + 1][j];
            if (j >= v[i])
                dp[i][j] = max(dp[i][j], dp[i + 1][j - v[i]] + w[i]);
        }
    int cur_v = m;
    for (int i = 1; i <= n; i++)
        if (cur_v >= v[i] && dp[i][cur_v] == dp[i + 1][cur_v - v[i]] + w[i]) {
            cout << i << ' ';
            cur_v -= v[i];
        }
}
```
