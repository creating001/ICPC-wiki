# 树上公共祖先

## 最近公共祖先

> 板子题网址: https://www.luogu.com.cn/problem/P3379

```cpp
inline void bfs(int root) {
    memset(dep, 0x3f, sizeof(dep));
    int hh = 0, tt = -1;
    dep[0] = 0, dep[root] = 1;
    q[++tt] = root;
    while (hh <= tt) {
        int cur = q[hh++];
        for (int i = h[cur]; i; i = nex[i]) {
            int to = e[i];
            if (dep[to] <= dep[cur] + 1) continue;
            q[++tt] = to;
            fa[to][0] = cur;
            dep[to] = dep[cur] + 1;
            for (int j = 1; j < S; j++)
                fa[to][j] = fa[fa[to][j - 1]][j - 1];
        }
    }
}

inline int lca(int a, int b) {
    if (a == b) return a;
    if (dep[a] < dep[b]) swap(a, b);
    for (int i = S - 1; i >= 0; i--)
        if (dep[fa[a][i]] >= dep[b]) a = fa[a][i];
    if (a == b) return a;
    for (int i = S - 1; i >= 0; i--)
        if (fa[a][i] != fa[b][i])
            a = fa[a][i], b = fa[b][i];
    return fa[a][0];
}
```

## 树链剖分

> 板子题网址: https://www.luogu.com.cn/problem/P3384

## 第K级祖先

> 板子题网址: https://www.luogu.com.cn/problem/P5903

```cpp
inline void bfs(int root) {
    memset(dep, 0x3f, sizeof(dep));
    int hh = 0, tt = -1;
    q[++tt] = root;
    dep[0] = 0, dep[root] = 1;
    while (hh <= tt) {
        int cur = q[hh++];
        for (int i = h[cur]; i; i = nex[i]) {
            int to = e[i];
            if (dep[to] <= dep[cur] + 1) continue;
            q[++tt] = to;
            fa[to][0] = cur;
            dep[to] = dep[cur] + 1;
            for (int j = 1; j < S; j++)
                fa[to][j] = fa[fa[to][j - 1]][j - 1];
        }
    }
}

inline int k_fa(int a, int k) {
    for (int i = S - 1; i >= 0; i--)
        if (k >> i & 1) a = fa[a][i];
    return a;
}
```
