# 树形DP

树形 DP，即在树上进行的 DP。由于树固有的递归性质，树形 DP 一般都是递归进行的。

## 基础树形DP

> 板子题网址: https://www.luogu.com.cn/problem/P1352

```cpp
inline void dfs(int u) {
    dp[u][1] = a[u];
    for (int i = h[u]; ~i; i = nex[i]) {
        int to = e[i];
        dfs(to);
        dp[u][0] += max(dp[to][1], dp[to][0]);
        dp[u][1] += max(dp[to][0], 0);
    }
}
```

## 树上背包

> 板子题网址: https://www.luogu.com.cn/problem/P2014

```cpp

```

## 换根DP

> 板子题网址: https://www.luogu.com.cn/problem/P3478

```cpp

```
