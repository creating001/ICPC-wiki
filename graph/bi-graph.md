# 二分图

## 染色法判断二分图

> 板子题链接: https://www.acwing.com/problem/content/862

```cpp
inline bool dfs(int u, int c) {
    color[u] = c;
    for (int i = h[u]; i; i = nex[i]) {
        int to = e[i];
        if (!color[to] && !dfs(to, 3 - c))
            return false;
        if (color[to] != 3 - c) return false;
    }
    return true;
}
```

## 匈牙利算法

> 板子题链接: https://www.luogu.com.cn/problem/P3386

```cpp
inline bool dfs(int u) {
    for (int i = h[u]; i; i = nex[i]) {
        int to = e[i];
        if (vis[to]) continue;
        vis[to] = 1;
        if (!match[to] || dfs(match[to])) {
            match[to] = u;
            return true;
        }
    }
    return false;
}
```
