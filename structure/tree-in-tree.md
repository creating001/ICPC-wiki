# 树套树

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

## 权值线段树套线段树

> 板子题网址: https://www.luogu.com.cn/problem/P3332

```cpp
struct node {
    int l, r, add; LL sum;
} tr[M];
int n, m;
int L[N << 2], R[N << 2], V[N << 2], idx;

inline void pushup(int u) {
    tr[u].sum = tr[tr[u].l].sum + tr[tr[u].r].sum;
}

inline void pushdown(int u, int l, int r) {
    if (!tr[u].add) return;
    if (!tr[u].l) tr[u].l = ++idx;
    if (!tr[u].r) tr[u].r = ++idx;
    node& c = tr[u], & ls = tr[c.l], & rs = tr[c.r];
    int mid = (l + r) >> 1;
    ls.sum += (mid - l + 1) * c.add, ls.add += c.add;
    rs.sum += (r - (mid + 1) + 1) * c.add, rs.add += c.add;
    c.add = 0;
}

inline void modify(int u, int l, int r, int ql, int qr) {
    tr[u].sum += min(qr, r) - max(l, ql) + 1;
    if (l >= ql && r <= qr) return tr[u].add++, void();
    pushdown(u, l, r);
    int mid = (l + r) >> 1;
    if (ql <= mid) {
        if (!tr[u].l) tr[u].l = ++idx;
        modify(tr[u].l, l, mid, ql, qr);
    }
    if (qr >= mid + 1) {
        if (!tr[u].r) tr[u].r = ++idx;
        modify(tr[u].r, mid + 1, r, ql, qr);
    }
    pushup(u);
}

inline LL get_sum(int u, int l, int r, int ql, int qr) {
    if (l >= ql && r <= qr) return tr[u].sum;
    pushdown(u, l, r);
    LL sum = 0;
    int mid = (l + r) >> 1;
    if (ql <= mid && tr[u].l) sum += get_sum(tr[u].l, l, mid, ql, qr);
    if (qr >= mid + 1 && tr[u].r) sum += get_sum(tr[u].r, mid + 1, r, ql, qr);
    return sum;
}

inline void build(int u, int l, int r) {
    L[u] = l, R[u] = r, V[u] = ++idx;
    if (l == r) return;
    int mid = (L[u] + R[u]) >> 1;
    build(u << 1, l, mid);
    build(u << 1 | 1, mid + 1, r);
}

inline void modify(int u, int l, int r, int c) {
    modify(V[u], 1, n, l, r);
    if (L[u] == R[u]) return;
    int mid = (L[u] + R[u]) >> 1;
    if (c <= mid) modify(u << 1, l, r, c);
    else modify(u << 1 | 1, l, r, c);
}

inline int query(int u, int l, int r, LL c) {
    if (L[u] == R[u]) return L[u];
    LL k = get_sum(V[u << 1 | 1], 1, n, l, r);
    if (k >= c) return query(u << 1 | 1, l, r, c);
    return query(u << 1, l, r, c - k);
}
```

## 树状数组套权值线段树

> 板子题网址: https://www.luogu.com.cn/problem/P2617

```cpp
struct node {
    int l, r, v;
} tr[M];
int n, root[N], idx, tmp[N][2], n1, n2, tot;

inline void modify(int& u, int l, int r, int c, int v) {
    if (!u) u = ++idx;
    tr[u].v += v;
    if (l == r) return;
    int mid = (l + r) >> 1;
    if (c <= mid) modify(tr[u].l, l, mid, c, v);
    else modify(tr[u].r, mid + 1, r, c, v);
}

inline void modify(int p, int num, int v) {
    for (int i = p; i <= n; i += lowbit(i))
        modify(root[i], 1, tot, num, v);
}

inline int query(int l, int r, int k) {
    if (l == r) return l;
    int sum = 0, mid = (l + r) >> 1;
    for (int i = 1; i <= n1; i++)
        sum -= tr[tr[tmp[i][0]].l].v;
    for (int i = 1; i <= n2; i++)
        sum += tr[tr[tmp[i][1]].l].v;
    if (sum >= k) {
        for (int i = 1; i <= n1; i++)
            tmp[i][0] = tr[tmp[i][0]].l;
        for (int i = 1; i <= n2; i++)
            tmp[i][1] = tr[tmp[i][1]].l;
        return query(l, mid, k);
    } else {
        for (int i = 1; i <= n1; i++)
            tmp[i][0] = tr[tmp[i][0]].r;
        for (int i = 1; i <= n2; i++)
            tmp[i][1] = tr[tmp[i][1]].r;
        return query(mid + 1, r, k - sum);
    }
}

inline int query_pre(int l, int r, int k) {
    n1 = n2 = 0;
    for (int i = l - 1; i; i -= lowbit(i))
        tmp[++n1][0] = root[i];
    for (int i = r; i; i -= lowbit(i))
        tmp[++n2][1] = root[i];
    return query(1, tot, k);
}
```
