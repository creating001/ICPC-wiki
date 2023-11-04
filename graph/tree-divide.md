# 树分治

## 点分治

> 板子题网址: https://www.luogu.com.cn/problem/P3806
> 板子题网址: https://www.luogu.com.cn/problem/P4149

```cpp
inline void dfs1(int u, int fa) {
    siz[u] = 1;
    for (int i = h[u]; i; i = nex[i]) {
        int to = e[i];
        if (vis[to] || to == fa) continue;
        dfs1(to, u);
        siz[u] += siz[to];
    }
}

inline void dfs2(int u, int fa, int tot, int& v) {
    int sum = 1, mx = 1;
    for (int i = h[u]; i; i = nex[i]) {
        int to = e[i];
        if (to == fa || vis[to]) continue;
        dfs2(to, u, tot, v);
        sum += siz[to], mx = max(mx, siz[to]);
    }
    mx = max(mx, tot - sum);
    if (mx <= tot / 2) v = u;
}

inline void dfs3(int u, int fa, int d, int cnt, int& qt) {
    if (d > k) return;
    q[qt++] = {d, cnt};
    for (int i = h[u]; i; i = nex[i]) {
        int to = e[i];
        if (vis[to] || to == fa) continue;
        dfs3(to, u, d + ew[i], cnt + 1, qt);
    }
}

inline void calc(int u) {
    dfs1(u, u);
    dfs2(u, u, siz[u], u);
    vis[u] = 1;
    int pt = 0;
    for (int i = h[u]; i; i = nex[i]) {
        int to = e[i];
        if (vis[to]) continue;
        int qt = 0;
        dfs3(to, u, ew[i], 1, qt);
        for (int j = 0; j < qt; j++) {
            if (q[j].F == k) ans = min(ans, q[j].S);
        }
        for (int j = 0; j < qt; j++) {
            if (f[k - q[j].F] != INF) ans = min(ans, f[k - q[j].F] + q[j].S);
        }
        for (int j = 0; j < qt; j++) {
            f[q[j].F] = min(f[q[j].F], q[j].S);
            p[pt++] = q[j];
        }
    }
    for (int i = 0; i < pt; i++) f[p[i].F] = INF;
    for (int i = h[u]; i; i = nex[i]) if (!vis[e[i]]) calc(e[i]);
}
```

## 点分树

> 板子题网址: https://www.luogu.com.cn/problem/P3241
