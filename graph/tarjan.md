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

## 强连通分量

> 板子题网址: https://www.luogu.com.cn/problem/B3609

```cpp

```

## 缩点

板子题网址: https://www.luogu.com.cn/problem/P3387

```cpp

```

## 点双连通分量

> 板子题网址: https://www.luogu.com.cn/problem/P8435

```cpp

```

## 边双连通分量

> 板子题网址: https://www.luogu.com.cn/problem/P8436

```cpp

```

## 割点

板子题网址: https://www.luogu.com.cn/problem/P3388

```cpp

```
