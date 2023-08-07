# DFS

## 有序枚举

> 板子题网址: https://www.luogu.com.cn/problem/AT_abc310_d

```cpp
inline void dfs(int u, int cnt) {
    if (cnt >= ans) return void();
    if (u == n) return (ans = min(ans, cnt), void());
    for (int i = 1; i <= cnt; i++)
        if (check(g[i], a[u])) {
            g[i].emplace_back(a[u]);
            dfs(u + 1, cnt);
            g[i].pop_back();
        }
    g[cnt + 1].emplace_back(a[u]);
    dfs(u + 1, cnt + 1);
    g[cnt + 1].pop_back();
}
```

## DFS剪枝

```cpp

```

## 迭代加深

```cpp

```

## 双向DFS

```cpp

```

## IDA*

```cpp

```
