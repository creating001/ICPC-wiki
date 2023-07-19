# 最小生成树

> 板子题链接: https://www.luogu.com.cn/problem/P3366

## Prim算法

> prim算法流程是: 选取一个点作为起点, 然后每次选取一个与当前集合距离最小的点加入集合, 直到所有点都加入集合为止。

时间复杂度: $O(N^2)$

```cpp
inline int prim() {
    int ans = 0;
    memset(dis, 0x3f, sizeof(dis));
    dis[1] = 0;
    for (int i = 1; i <= n; i++) {
        int u = -1;
        for (int j = 1; j <= n; j++)
            if (!vis[j] && (u == -1 || dis[j] < dis[u])) u = j;
        if (dis[u] == inf) return inf;
        ans += dis[u], vis[u] = 1;
        for (int j = 1; j <= n; j++)
            if (!vis[j]) dis[j] = min(dis[j], g[j][u]);
    }
    return ans;
}
```

## Kruskal算法

> kruskal 算法流程是: 每次选取一条最短的边, 如果这条边的两个端点不在同一个集合中, 则将这条边加入最小生成树中, 直到最小生成树中有 $n-1$ 条边为止。

时间复杂度: $O(M \log M)$

```cpp
inline int kruskal() {
    int ans = 0;
    for (int i = 0; i < m; i++) {
        int px = find(e[i].x), py = find(e[i].y);
        if (px != py) p[px] = py, ans += e[i].w;
    }
    for (int i = 1; i < n; i++)
        if (find(i) != find(i + 1)) return inf;
    return ans;
}
```
