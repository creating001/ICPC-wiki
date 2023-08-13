# 环

## 无向图最小环

> 板子题网址: https://www.luogu.com.cn/problem/P6175

```cpp

```

## 负环

> 板子题网址: https://www.luogu.com.cn/problem/P3385

### Bellman-Ford

```cpp
inline bool bellman_ford(int s) {
    memset(dis, 0x3f, sizeof(dis));
    dis[s] = 0;
    int flag = false;
    for (int i = 1; i <= n; i++){
        flag = false;
        for (int a = 1; a <= n; a++)
            for (int b = h[a]; b; b = nex[b]) {
                int from = a, to = e[b], c = w[b];
                if (dis[a] == INF) continue;
                if (dis[to] > dis[from] + c)
                    dis[to] = dis[from] + c, flag = true;
            }
        if (!flag) break;
    }
    return flag;
}
```

### SPFA

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
                cnt[to] = cnt[cur] + 1;
                if (cnt[to] >= n) return true;
                if (!vis[to]) q.emplace(to), vis[to] = 1;
            }
        }
    }
    return false;
}
```
