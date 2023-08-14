# 最短路算法

## Dijkstra算法

> 板子题网址: https://www.luogu.com.cn/problem/P4779

### 朴素Dijkstra算法

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

```cpp
inline int dijkstra(int s, int t) {
    memset(dis, 0x3f, sizeof dis);
    priority_queue<PII, vector<PII>, greater<PII>> q;
    q.emplace(0, s);
    dis[s] = 0;
    while (!q.empty()) {
        int cur = q.top().S;
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

> 板子题网址: https://www.acwing.com/problem/content/855

```cpp
inline int bellman_ford(int s, int t) {
    memset(dis, 0x3f, sizeof dis);
    dis[s] = 0;
    for (int i = 1; i <= k; i++) {
        memcpy(tmp, dis, sizeof dis);
        for (int a = 1; a <= n; a++)
            for (int b = h[a]; b; b = nex[b]) {
                int from = a, to = e[b], c = w[b];
                if (tmp[from] == inf) continue;
                dis[to] = min(dis[to], tmp[from] + c);
            }
    }
    return dis[t];
}
```

## Floyd算法

### 朴素Floyd

> 板子题网址: https://www.luogu.com.cn/problem/B3647

```cpp
inline void floyd() {
    for (int k = 1; k <= n; k++)
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= n; j++) {
                if (dis[i][k] == inf || dis[k][j] == inf) continue;
                dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
            }
}
```

## 扩展Floyd

> 板子题网址: https://www.luogu.com.cn/problem/P2886

```cpp
inline void mul(int a[][N], int b[][N], int c[][N]) {
    static int tmp[N][N];
    memset(tmp, 0x3f, sizeof(tmp));
    for (int k = 0; k < n; k++)
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                tmp[i][j] = min(tmp[i][j], b[i][k] + c[k][j]);
    memcpy(a, tmp, sizeof(tmp));
}

inline void qpow(int k) {
    memset(ans, 0x3f, sizeof(ans));
    for (int i = 0; i < n; i++) ans[i][i] = 0;
    while (k) {
        if (k & 1) mul(ans, ans, g);
        mul(g, g, g);
        k >>= 1;
    }
}
```

## Johnson算法

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

## 差分约束系统

> 板子题网址: https://www.luogu.com.cn/problem/P5960

## 拓扑排序

> 板子题网址: https://www.luogu.com.cn/problem/B3644

```cpp
inline bool topo_sort() {
    queue<int> q;
    for (int i = 1; i <= n; i++)
        if (!d[i]) q.emplace(i), vis[i] = 1;
    while (!q.empty()) {
        int cur = q.front();
        q.pop();
        a[cnts++] = cur;
        for (int i = h[cur]; i; i = nex[i]) {
            int to = e[i];
            if (vis[to]) continue;
            d[to]--;
            if (!d[to]) q.emplace(to), vis[to] = 1;
        }
    }
    return cnts == n;
}
```
