# Tarjan

## Tarjan-LCA

> 板子题网址: https://www.luogu.com.cn/problem/P5838

```cpp
inline void tarjan(int u) {
    vis[u] = 1;
    for (int i = h[u]; i; i = nex[i]) {
        int to = e[i];
        if (vis[to]) continue;
        tarjan(to);
        p[find(to)] = u;
    }
    for (auto& o : q[u]) {
        int v = o.F, id = o.S;
        if (vis[v] != 2) continue;
        int fa = find(v);
        ans[id] = d[u] + d[v] - 2 * d[fa];
    }
    vis[u] = 2;
}
```
