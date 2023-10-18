# LCA

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

```cpp
struct Segment_Tree {
    int l, r, sum, add;
} tr[N << 2];
int h[N], e[M], nex[M], idx = 1;
int n, m, root, P, a[N], id[N], top[N], dep[N], fa[N], siz[N], son[N], w[N], cnts;

inline void add_edge(int x, int y) {
    e[idx] = y, nex[idx] = h[x], h[x] = idx++;
}

inline void dfs1(int u, int p, int d) {
    siz[u] = 1, fa[u] = p, dep[u] = d;
    for (int i = h[u]; i; i = nex[i]) {
        int to = e[i];
        if (to == p) continue;
        dfs1(to, u, d + 1);
        siz[u] += siz[to];
        if (siz[son[u]] < siz[to]) son[u] = to;
    }
}

inline void dfs2(int u, int t) {
    id[u] = ++cnts, w[id[u]] = a[u], top[u] = t;
    if (!son[u]) return;
    dfs2(son[u], t);
    for (int i = h[u]; i; i = nex[i]) {
        int to = e[i];
        if (to == fa[u] || to == son[u]) continue;
        dfs2(to, to);
    }
}

inline void pushup(int u) {
    tr[u].sum = tr[u << 1].sum + tr[u << 1 | 1].sum;
}

inline void pushdown(int u) {
    if (!tr[u].add) return;
    auto& c = tr[u], & l = tr[u << 1], & r = tr[u << 1 | 1];
    l.sum += c.add * (l.r - l.l + 1), l.add += c.add;
    r.sum += c.add * (r.r - r.l + 1), r.add += c.add;
    c.add = 0;
}

inline void build(int u, int l, int r) {
    tr[u].l = l, tr[u].r = r;
    if (l == r) return tr[u].sum = w[l], void();
    int mid = (tr[u].l + tr[u].r) >> 1;
    build(u << 1, l, mid);
    build(u << 1 | 1, mid + 1, r);
    pushup(u);
}

inline void modify(int u, int l, int r, int k) {
    if (tr[u].l >= l && tr[u].r <= r) {
        tr[u].add += k;
        tr[u].sum += (tr[u].r - tr[u].l + 1) * k;
        return;
    }
    pushdown(u);
    int mid = (tr[u].l + tr[u].r) >> 1;
    if (l <= mid) modify(u << 1, l, r, k);
    if (r >= mid + 1) modify(u << 1 | 1, l, r, k);
    pushup(u);
}

inline int query(int u, int l, int r) {
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].sum;
    pushdown(u);
    int sum = 0;
    int mid = (tr[u].l + tr[u].r) >> 1;
    if (l <= mid) sum += query(u << 1, l, r);
    if (r >= mid + 1) sum += query(u << 1 | 1, l, r);
    return sum;
}

inline int query_tree(int u) {
    return query(1, id[u], id[u] + siz[u] - 1);
}

inline void modify_tree(int u, int k) {
    modify(1, id[u], id[u] + siz[u] - 1, k);
}

inline int query_path(int u, int v) {
    int sum = 0;
    while (top[u] != top[v]) {
        if (dep[top[u]] < dep[top[v]]) swap(u, v);
        sum += query(1, id[top[u]], id[u]);
        u = fa[top[u]];
    }
    if (dep[u] < dep[v]) swap(u, v);
    sum += query(1, id[v], id[u]);
    return sum;
}

inline void modify_path(int u, int v, int k) {
    while (top[u] != top[v]) {
        if (dep[top[u]] < dep[top[v]]) swap(u, v);
        modify(1, id[top[u]], id[u], k);
        u = fa[top[u]];
    }
    if (dep[u] < dep[v]) swap(u, v);
    modify(1, id[v], id[u], k);
}
```
