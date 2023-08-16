# Tarjan

## 离线求LCA

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

## 强连通分量和缩点

> 板子题网址: https://www.luogu.com.cn/problem/B3609
>
> 板子题网址: https://www.luogu.com.cn/problem/P3387

```cpp
inline void tarjan(int u) {
    dfn[u] = low[u] = ++tot;
    stk[++top] = u, ins[u] = 1;
    for (int i = h[u]; i; i = nex[i]) {
        int to = e[i];
        if (dfn[to]) {
            if (ins[to]) low[u] = min(low[u], dfn[to]);
            continue;
        }
        tarjan(to);
        low[u] = min(low[u], low[to]);
    }
    if (dfn[u] == low[u]) {
        scc++;
        int v;
        do {
            v = stk[top--];
            ins[v] = 0;
            id[v] = scc;
        } while (v != u);
    }
}
```


## 边双连通分量

> 板子题网址: https://www.luogu.com.cn/problem/P8436

```cpp
inline void tarjan(int u, int fa) {
    dfn[u] = low[u] = ++tot;
    stk[++top] = u;
    for (int i = h[u]; ~i; i = nex[i]) {
        int to = e[i];
        if (dfn[to]) {
            if (i != (fa ^ 1)) low[u] = min(low[u], dfn[to]);
            continue;
        }
        tarjan(to, i);
        low[u] = min(low[u], low[to]);
        if (dfn[u] < low[to]) vis[i] = vis[i ^ 1] = true;
    }
    if (dfn[u] == low[u]) {
        dcc++;
        int v;
        do {
            v = stk[top--];
            id[v] = dcc;
        } while (v != u);
    }
}
```

## 割点

板子题网址: https://www.luogu.com.cn/problem/P3388

```cpp
inline void tarjan(int u) {
    dfn[u] = low[u] = ++tot;
    int cnt = 0;
    for (int i = h[u]; i; i = nex[i]) {
        int to = e[i];
        if (dfn[to]) {
            low[u] = min(low[u], dfn[to]);
            continue;
        }
        tarjan(to);
        low[u] = min(low[u], low[to]);
        if (low[to] >= dfn[u]) {
            cnt++;
            if (u != root || cnt > 1) cut[u] = 1;
        }
    }
}
```

## 点双连通分量

> 板子题网址: https://www.luogu.com.cn/problem/P8435

```cpp
inline void tarjan(int u) {
    dfn[u] = low[u] = ++tot;
    stk[++top] = u;

    if (u == root && h[u] == 0)
        return g[++dcc].emplace_back(u), void();

    int cnt = 0;
    for (int i = h[u]; i; i = nex[i]) {
        int to = e[i];
        if (dfn[to]) {
            low[u] = min(low[u], dfn[to]);
            continue;
        }
        tarjan(to);
        low[u] = min(low[u], low[to]);
        if (dfn[u] <= low[to]) {
            cnt++;
            if (u != root || cnt > 1) cut[u] = 1;
            dcc++;
            int v;
            do {
                v = stk[top--];
                g[dcc].emplace_back(v);
            } while (v != to);
            g[dcc].emplace_back(u);
        }
    }
}
```
