# 树套树

## 线段树套线段树

> 板子题网址: https://www.luogu.com.cn/problem/P3332

```cpp

```

## 线段树套平衡树

> 板子题网址: https://www.luogu.com.cn/problem/P3380

```cpp
struct Splay {
    int s[2], p, v;
    int size;
} tr[S];
int a[N];
int L[M], R[M], V[M], idx;

inline void pushup(int u) {
    tr[u].size = tr[tr[u].s[0]].size + tr[tr[u].s[1]].size + 1;
}

inline void rotate(int u) {
    int y = tr[u].p, z = tr[y].p;
    int k = tr[y].s[1] == u;
    tr[z].s[tr[z].s[1] == y] = u, tr[u].p = z;
    tr[y].s[k] = tr[u].s[k ^ 1], tr[tr[u].s[k ^ 1]].p = y;
    tr[u].s[k ^ 1] = y, tr[y].p = u;
    pushup(y), pushup(u);
}

inline void splay(int& root, int u, int v) {
    while (tr[u].p != v) {
        int y = tr[u].p, z = tr[y].p;
        if (z != v) {
            if ((tr[z].s[1] == y) != (tr[y].s[1] == u)) rotate(u);
            else rotate(y);
        }
        rotate(u);
    }
    if (!v) root = u;
}

inline void insert(int& root, int x) {
    int u = root, p = 0;
    while (u) p = u, u = tr[u].s[x > tr[u].v];
    u = ++idx;
    if (p) tr[p].s[x > tr[p].v] = u;
    tr[u].p = p, tr[u].v = x, tr[u].size = 1;
    splay(root, u, 0);
}

inline void find(int& root, int x) {
    int u = root;
    while (tr[u].v != x && tr[u].s[x > tr[u].v])
        u = tr[u].s[x > tr[u].v];
    splay(root, u, 0);
}

inline int get_pre(int& root, int x) {
    int u = root, ans = 0;
    while (u) {
        if (tr[u].v < x) ans = u, u = tr[u].s[1];
        else u = tr[u].s[0];
    }
    return ans;
}

inline int get_nex(int& root, int x) {
    int u = root, ans = 0;
    while (u) {
        if (tr[u].v > x) ans = u, u = tr[u].s[0];
        else u = tr[u].s[1];
    }
    return ans;
}

inline int get_rank(int& root, int x) {
    int u = root, ans = 0;
    while (u) {
        if (tr[u].v < x) ans += tr[tr[u].s[0]].size + 1, u = tr[u].s[1];
        else u = tr[u].s[0];
    }
    return ans;
}

inline void change(int& root, int x, int y) {
    find(root, x);
    int u = root;
    int l = tr[u].s[0], r = tr[u].s[1];
    while (tr[l].s[1]) l = tr[l].s[1];
    while (tr[r].s[0]) r = tr[r].s[0];
    splay(root, l, 0), splay(root, r, l);
    tr[r].s[0] = 0;
    pushup(r), pushup(l);
    insert(root, y);
}

inline void build(int u, int l, int r) {
    L[u] = l, R[u] = r;
    insert(V[u], -INF), insert(V[u], INF);
    for (int i = l; i <= r; i++) insert(V[u], a[i]);
    if (l == r) return;
    int mid = (L[u] + R[u]) >> 1;
    build(u << 1, l, mid);
    build(u << 1 | 1, mid + 1, r);
}

inline int query_rank(int u, int l, int r, int x) {
    if (L[u] >= l && R[u] <= r) {
        return get_rank(V[u], x) - 1;
    }
    int ans = 0, mid = (L[u] + R[u]) >> 1;
    if (l <= mid) ans += query_rank(u << 1, l, r, x);
    if (r >= mid + 1) ans += query_rank(u << 1 | 1, l, r, x);
    return ans;
}

inline void modify(int u, int p, int x) {
    change(V[u], a[p], x);
    if (L[u] == R[u]) return;
    int mid = (L[u] + R[u]) >> 1;
    if (p <= mid) modify(u << 1, p, x);
    else modify(u << 1 | 1, p, x);
}

inline int query_pre(int u, int l, int r, int x) {
    if (L[u] >= l && R[u] <= r) {
        return tr[get_pre(V[u], x)].v;
    }
    int mid = (L[u] + R[u]) >> 1, ans = -INF;
    if (l <= mid) ans = max(ans, query_pre(u << 1, l, r, x));
    if (r >= mid + 1) ans = max(ans, query_pre(u << 1 | 1, l, r, x));
    return ans;
}

inline int query_nex(int u, int l, int r, int x) {
    if (L[u] >= l && R[u] <= r) {
        return tr[get_nex(V[u], x)].v;
    }
    int mid = (L[u] + R[u]) >> 1, ans = INF;
    if (l <= mid) ans = min(ans, query_nex(u << 1, l, r, x));
    if (r >= mid + 1) ans = min(ans, query_nex(u << 1 | 1, l, r, x));
    return ans;
}
```

## 整体二分套线段树

> 板子题网址: https://www.luogu.com.cn/problem/P2617

```cpp

```
