# Splay

## Splay平衡树

> 板子题网址: https://www.luogu.com.cn/problem/P6136

```cpp

```

## Splay线段树

> 板子题网址: https://www.luogu.com.cn/problem/P3391

```cpp
struct Splay {
    int s[2], p, v;
    int size, flag;
} tr[N];
int n, m, root, idx;

inline void pushup(int u) {
    tr[u].size = tr[tr[u].s[0]].size + tr[tr[u].s[1]].size + 1;
}

inline void pushdown(int u) {
    if (!tr[u].flag) return;
    swap(tr[u].s[0], tr[u].s[1]);
    tr[tr[u].s[0]].flag ^= 1;
    tr[tr[u].s[1]].flag ^= 1;
    tr[u].flag = 0;
}

inline void rotate(int u) {
    int y = tr[u].p, z = tr[y].p;
    int k = tr[y].s[1] == u;
    tr[z].s[tr[z].s[1] == y] = u, tr[u].p = z;
    tr[y].s[k] = tr[u].s[k ^ 1], tr[tr[u].s[k ^ 1]].p = y;
    tr[u].s[k ^ 1] = y, tr[y].p = u;
    pushup(y), pushup(u);
}

inline void splay(int u, int v) {
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

inline int get_k(int k) {
    int u = root;
    while (true) {
        pushdown(u);
        if (tr[tr[u].s[0]].size >= k) u = tr[u].s[0];
        else if (tr[tr[u].s[0]].size + 1 == k) return u;
        else k -= tr[tr[u].s[0]].size + 1, u = tr[u].s[1];
    }
}

inline void insert(int x) {
    int u = root, p = 0;
    while (u) p = u, u = tr[u].s[x > tr[u].v];
    u = ++idx;
    if (p) tr[p].s[x > tr[p].v] = u;
    tr[u].p = p, tr[u].v = x;
    splay(u, 0);
}

inline void print(int u) {
    pushdown(u);
    if (tr[u].s[0]) print(tr[u].s[0]);
    if (tr[u].v >= 1 && tr[u].v <= n) cout << tr[u].v << " ";
    if (tr[u].s[1]) print(tr[u].s[1]);
}
```

## Splay终极

> 板子题网址: https://www.luogu.com.cn/problem/P2042

```cpp
struct Splay {
    int s[2], p, v;
    int same, rev, size, sum, ms, ls, rs;
    inline void init(int _v, int _p) {
        s[0] = s[1] = 0;
        v = _v, p = _p;
        same = rev = 0, size = 1;
        sum = ms = v, ls = rs = max(v, 0);
    }
} tr[N];
int stk[N], top, root, a[N];

inline void pushup(int u) {
    auto& c = tr[u], & l = tr[tr[u].s[0]], & r = tr[tr[u].s[1]];
    c.size = l.size + r.size + 1;
    c.sum = l.sum + r.sum + c.v;
    c.ls = max(l.ls, l.sum + c.v + r.ls);
    c.rs = max(r.rs, r.sum + c.v + l.rs);
    c.ms = max(l.rs + c.v + r.ls, max(l.ms, r.ms));
}

inline void pushdown(int u) {
    auto& c = tr[u], & l = tr[tr[u].s[0]], & r = tr[tr[u].s[1]];
    if (c.same) {
        c.same = c.rev = 0;
        if (c.s[0]) l.same = 1, l.v = c.v, l.sum = l.v * l.size;
        if (c.s[1]) r.same = 1, r.v = c.v, r.sum = r.v * r.size;
        if (c.v > 0) {
            if (c.s[0]) l.ls = l.rs = l.ms = l.sum;
            if (c.s[1]) r.ls = r.rs = r.ms = r.sum;
        } else {
            if (c.s[0]) l.ls = l.rs = 0, l.ms = l.v;
            if (c.s[1]) r.ls = r.rs = 0, r.ms = r.v;
        }
    } else if (c.rev) {
        c.rev = 0;
        l.rev ^= 1, swap(l.s[0], l.s[1]), swap(l.ls, l.rs);
        r.rev ^= 1, swap(r.s[0], r.s[1]), swap(r.ls, r.rs);
    }
}

inline void rotate(int u) {
    int y = tr[u].p, z = tr[y].p;
    int k = tr[y].s[1] == u;
    tr[z].s[tr[z].s[1] == y] = u, tr[u].p = z;
    tr[y].s[k] = tr[u].s[k ^ 1], tr[tr[u].s[k ^ 1]].p = y;
    tr[u].s[k ^ 1] = y, tr[y].p = u;
    pushup(y), pushup(u);
}

inline void splay(int u, int v) {
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

inline int build(int l, int r, int p) {
    int mid = (l + r) >> 1;
    int u = stk[top--];
    tr[u].init(a[mid], p);
    if (l < mid) tr[u].s[0] = build(l, mid - 1, u);
    if (r > mid) tr[u].s[1] = build(mid + 1, r, u);
    pushup(u);
    return u;
}

inline int get_k(int k) {
    int u = root;
    while (true) {
        pushdown(u);
        if (k <= tr[tr[u].s[0]].size) u = tr[u].s[0];
        else if (k == tr[tr[u].s[0]].size + 1) return u;
        else k -= tr[tr[u].s[0]].size + 1, u = tr[u].s[1];
    }
}

inline void gc(int u) {
    if (tr[u].s[0]) gc(tr[u].s[0]);
    if (tr[u].s[1]) gc(tr[u].s[1]);
    stk[++top] = u;
}
```
