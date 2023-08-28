# 线段树

## 线段树基础

> 板子题网址: https://www.luogu.com.cn/problem/P3373

```cpp
inline void pushup(int u) {
    tr[u].s = (tr[u << 1].s + tr[u << 1 | 1].s) % P;
}

inline void pushdown(int u) {
    node& c = tr[u], & l = tr[u << 1], & r = tr[u << 1 | 1];
    l.k = (c.k * l.k) % P;
    l.b = (l.b * c.k + c.b) % P;
    l.s = (l.s * c.k + c.b * (l.r - l.l + 1)) % P;
    r.k = (c.k * r.k) % P;
    r.b = (r.b * c.k + c.b) % P;
    r.s = (r.s * c.k + c.b * (r.r - r.l + 1)) % P;
    c.k = 1, c.b = 0;
}

inline void build(int u, int l, int r) {
    tr[u].l = l, tr[u].r = r, tr[u].k = 1, tr[u].b = 0;
    if (l == r) {
        tr[u].s = a[l];
        return;
    }
    int mid = (tr[u].l + tr[u].r) >> 1;
    build(u << 1, l, mid);
    build(u << 1 | 1, mid + 1, r);
    pushup(u);
}

inline void modify(int u, int l, int r, LL k, LL b) {
    if (tr[u].r <= r && tr[u].l >= l) {
        tr[u].k = (tr[u].k * k) % P;
        tr[u].b = (tr[u].b * k + b) % P;
        tr[u].s = (tr[u].s * k + b * (tr[u].r - tr[u].l + 1)) % P;
        return;
    }
    pushdown(u);
    int mid = (tr[u].l + tr[u].r) >> 1;
    if (l <= mid) modify(u << 1, l, r, k, b);
    if (r >= mid + 1) modify(u << 1 | 1, l, r, k, b);
    pushup(u);
}

inline LL query(int u, int l, int r) {
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].s;
    pushdown(u);
    int mid = (tr[u].l + tr[u].r) >> 1;
    LL ans = 0;
    if (l <= mid) ans = (ans + query(u << 1, l, r)) % P;
    if (r >= mid + 1) ans = (ans + query(u << 1 | 1, l, r)) % P;
    return ans;
}
```

## 主席树

> 板子题网址: https://www.luogu.com.cn/problem/P3834

```cpp

```

## 线段树合并

> 板子题网址: https://www.luogu.com.cn/problem/P4556

```cpp

```

## 线段树分治

> 板子题网址: https://www.luogu.com.cn/problem/P5787

```cpp

```
