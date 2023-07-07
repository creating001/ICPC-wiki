# 最短路算法

## Dijkstra算法

> Dijkstra算法用于解决非负权边的最短路问题。
>
> 板子题网址: https://www.luogu.com.cn/problem/P3371
>
> 板子题网址: https://www.luogu.com.cn/problem/P4779

### 朴素Dijkstra算法

> 朴素Dijkstra算法适用于稠密图, 时间复杂度为$O(V^2)$

```cpp
const int N = 1e3 + 10, INF = 0x3f3f3f3f;
int n, m;
int g[N][N], vis[N], dis[N];

inline int dijkstra(int s, int t) {
    memset(dis, 0x3f, sizeof(dis));
    dis[s] = 0;
    for (int i = 1; i <= n; i++) {
        int u = -1;
        for (int j = 1; j <= n; j++)
            if (!vis[j] && (u == -1 || dis[j] < dis[u])) u = j;
        if (u == -1) break;
        for (int j = 1; j <= n; j++)
            dis[j] = min(dis[j], dis[u] + g[u][j]);
        vis[u] = 1;
    }
    if (dis[t] == INF) return -1;
    return dis[t];
}
```

### 堆优化Dijkstra算法

> 堆优化Dijkstra算法适用于非稠密图, 时间复杂度为$O(ElogV)$。

```cpp

```

## Bellman-Ford算法

> Bellman-Ford算法用于解决负权边的最短路问题，其时间复杂度为$O(VE)$。
>
> 板子题网址: https://www.acwing.com/problem/content/855

```cpp

```

## SPFA算法

> SPFA算法是Bellman-Ford算法的队列优化，其时间复杂度为$O(kE)$, $k$为最短路的平均长度。
>
> 板子题网址: https://www.acwing.com/problem/content/853
> 板子题网址: https://www.luogu.com.cn/problem/P3385

```cpp

```


## Floyd算法

> Floyd算法用于解决稠密图的最短路问题，其时间复杂度为$O(V^3)$。
>
> 板子题网址: https://www.luogu.com.cn/problem/B3647

```cpp

```

## Johnson算法

> Johnson算法用于解决稀疏图的最短路问题，其时间复杂度为$O(V^2logV+VE)$。
>
> 板子题网址: https://www.luogu.com.cn/problem/P5905

```cpp

```
