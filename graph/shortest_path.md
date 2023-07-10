# 最短路算法

## Dijkstra算法

> Dijkstra算法用于解决非负权边的最短路问题。
>
> 板子题网址: https://www.luogu.com.cn/problem/P4779

### 朴素Dijkstra算法

> 朴素Dijkstra算法适用于稠密图, 时间复杂度为$O(V^2)$

```cpp
inline int dijkstra(int s, int t) {
    memset(dis, 0x3f, sizeof(dis));
    dis[s] = 0;
    for (int i = 1; i <= n; i++) {
        int u = -1;
        for (int j = 1; j <= n; j++)
            if (!vis[j] && (u == -1 || dis[j] < dis[u])) u = j;
        if (u == -1 || u == t) break;
        for (int j = 1; j <= n; j++)
            dis[j] = min(dis[j], dis[u] + g[u][j]);
        vis[u] = 1;
    }
    return dis[t];
}
```

### 堆优化Dijkstra算法

> 堆优化Dijkstra算法适用于非稠密图, 时间复杂度为$O(ElogV)$。

```cpp
inline int dijkstra(int s, int t) {
    memset(dis, 0x3f, sizeof dis);
    priority_queue<PII, vector<PII>, greater<PII>> q;
    q.emplace(0, s);
    dis[s] = 0;
    while (!q.empty()) {
        int cur = q.top().second;
        q.pop();
        if (vis[cur]) continue;
        vis[cur] = 1;
        for (int i = h[cur]; ~i; i = nex[i]) {
            int to = e_to[i], w = e_w[i];
            if (dis[to] > dis[cur] + w) {
                dis[to] = dis[cur] + w;
                q.emplace(dis[to], to);
            }
        }
    }
    return dis[t];
}
```

## Bellman-Ford算法

> Bellman-Ford算法用于解决负权边的最短路问题，其时间复杂度为$O(VE)$。

> 板子题网址: https://www.acwing.com/problem/content/855

```cpp
inline int bellman_ford(int s, int t) {
    memset(dis, 0x3f, sizeof(dis));
    dis[s] = 0;
    for (int i = 1; i <= k; i++) {
        memcpy(backup, dis, sizeof(dis));
        for (int j = 1; j <= m; j++) {
            auto& e = edge[j];
            if (backup[e.x] == INF) continue;
            if (dis[e.y] > backup[e.x] + e.w)
                dis[e.y] = backup[e.x] + e.w;
        }
    }
    return dis[t];
}
```

> 板子题网址: https://www.luogu.com.cn/problem/P3385

```cpp
inline bool bellman_ford(int s) {
    memset(dis, 0x3f, sizeof(dis));
    dis[s] = 0;
    for (int i = 1; i <= n - 1; i++)
        for (int j = 0; j < m; j++) {
            const auto& e = edges[j];
            if (dis[e.x] == INF) continue;
            if (dis[e.y] > dis[e.x] + e.w) {
                dis[e.y] = dis[e.x] + e.w;
            }
        }
    for (int i = 0; i < m; i++) {
        const auto& e = edges[i];
        if (dis[e.y] == INF || dis[e.x] == INF) continue;
        if (dis[e.y] > dis[e.x] + e.w) return true;
    }
    return false;
}
```

## SPFA算法

> SPFA算法是Bellman-Ford算法的队列优化，其时间复杂度为$O(kE)$, $k$为最短路的平均长度。
>
> 板子题网址: https://www.acwing.com/problem/content/853

```cpp
inline int spfa(int s, int t) {
    memset(dis, 0x3f, sizeof(dis));
    queue<int> q;
    q.push(s);
    vis[s] = 1, dis[s] = 0;
    while (!q.empty()) {
        int cur = q.front();
        q.pop(), vis[cur] = 0;
        if (dis[cur] == INF) continue;
        for (int i = h[cur]; ~i; i = nex[i]) {
            int to = e_to[i], w = e_w[i];
            if (dis[to] > dis[cur] + w) {
                dis[to] = dis[cur] + w;
                if (!vis[to]) q.emplace(to), vis[to] = 1;
            }
        }
    }
    return dis[t];
}
```

> 板子题网址: https://www.luogu.com.cn/problem/P3385

```cpp
inline bool spfa() {
    memset(dis, 0x3f, sizeof(dis));
    queue<int> q;
    q.emplace(1), vis[1] = 1, dis[1] = 0;
    while (!q.empty()) {
        int cur = q.front();
        q.pop();
        vis[cur] = 0;
        for (int i = h[cur]; ~i; i = nex[i]) {
            int to = e_to[i], w = e_w[i];
            if (dis[to] > dis[cur] + w) {
                dis[to] = dis[cur] + w;
                cnts[to] = cnts[cur] + 1;
                if (cnts[to] >= n) return true;
                if (!vis[to]) q.emplace(to), vis[to] = 1;
            }
        }
    }
    return false;
}
```

## Floyd算法

> Floyd算法用于解决稠密图的最短路问题，其时间复杂度为$O(V^3)$。
>
> 板子题网址: https://www.luogu.com.cn/problem/B3647

```cpp
inline void floyd() {
    for (int k = 1; k <= n; k++)
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= n; j++)
                dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
}
```

## Johnson算法

> Johnson算法用于解决稀疏图的最短路问题，其时间复杂度为$O(V^2logV+VE)$。
>
> 板子题网址: https://www.luogu.com.cn/problem/P5905

```cpp
inline bool johnson() {
    for (int i = 1; i <= n; i++) add_edge(0, i, 0);
    if (spfa(0)) return true;
    for (int i = 1; i <= n; i++)
        for (int j = h[i]; ~j; j = nex[j])
            e_w[j] += d[i] - d[e_to[j]];
    for (int i = 1; i <= n; i++) dijkstra(i);
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++){
            if (dis[i][j] == INF) continue;
            else dis[i][j] += d[j] - d[i];
        }
    return false;
}
```
