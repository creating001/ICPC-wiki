# 生成树


## 最小生成树

> 板子题链接: https://www.luogu.com.cn/problem/P3366

### Prim算法

```cpp
inline int prim() {
    memset(d, 0x3f, sizeof(d));
    d[1] = 0;
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        int u = -1;
        for (int j = 1; j <= n; j++)
            if (!vis[j] && (u == -1 || d[j] < d[u])) u = j;
        if (d[u] == INF) return INF;
        ans += d[u], vis[u] = 1;
        for (int j = 1; j <= n; j++) d[j] = min(d[j], g[u][j]);
    }
    return ans;
}
```

### Kruskal算法

```cpp
inline int kruskal() {
    int ans = 0;
    for (int i = 0; i < m; i++) {
        int px = find(e[i].x), py = find(e[i].y);
        if (px != py) p[px] = py, ans += e[i].w;
    }
    for (int i = 1; i < n; i++)
        if (find(i) != find(i + 1)) return INF;
    return ans;
}
```

## 次小生成树

> 板子题网址: https://www.luogu.com.cn/problem/P4180

```cpp
inline int lca(int a, int b, int w) {
    static int dis[N * 2];
    int cnt = 0;
    if (dep[a] < dep[b]) swap(a, b);
    for (int i = S - 1; i >= 0; i--) {
        if (dep[fa[a][i]] >= dep[b]) {
            dis[cnt++] = d1[a][i];
            dis[cnt++] = d2[a][i];
            a = fa[a][i];
        }
    }
    for (int i = S - 1; i >= 0; i--) {
        if (fa[a][i] != fa[b][i]) {
            dis[cnt++] = d1[a][i];
            dis[cnt++] = d2[a][i];
            dis[cnt++] = d1[b][i];
            dis[cnt++] = d2[b][i];
            a = fa[a][i], b = fa[b][i];
        }
    }
    if (a != b) {
        dis[cnt++] = d1[a][0];
        dis[cnt++] = d1[b][0];
    }
    int ans1 = -INF, ans2 = -INF;
    for (int i = 0; i < cnt; i++) {
        if (dis[i] > ans1) ans2 = ans1, ans1 = dis[i];
        if (dis[i] != ans1 && dis[i] > ans2) ans2 = dis[i];
    }
    if (w > ans1) return w - ans1;
    if (w > ans2) return w - ans2;
    return INF;
}
```
