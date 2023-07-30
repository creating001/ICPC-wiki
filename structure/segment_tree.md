# 线段树

## 线段树基础

> 板子题网址: https://www.luogu.com.cn/problem/P3372
>
> 板子题网址: https://www.luogu.com.cn/problem/P3373

```cpp
struct node{};

inline node pushup(node& a, node& b) {

}

inline void pushup(int u) {

}

inline void pushdown(int u) {

}

inline void build(int u, int l, int r) {

}

inline void modify(int u, int l, int r, int d) {

}

inline node query(int u, int l, int r) {

}
```

## 扫描线

> 板子题网址: https://www.luogu.com.cn/problem/P5490

```cpp
inline void pushup(int u) {
    if (tr[u].cnt) {
        tr[u].len = a[tr[u].r + 1] - a[tr[u].l];
    } else if (tr[u].l != tr[u].r) {
        tr[u].len = tr[u << 1].len + tr[u << 1 | 1].len;
    } else {
        tr[u].len = 0;
    }
}

inline void build(int u, int l, int r) {
    tr[u].l = l, tr[u].r = r;
    if (l == r) return;
    int mid = (tr[u].l + tr[u].r) >> 1;
    build(u << 1, l, mid);
    build(u << 1 | 1, mid + 1, r);
}

inline void modify(int u, int l, int r, int k) {
    if (tr[u].l >= l && tr[u].r <= r) {
        tr[u].cnt += k;
        return pushup(u);
    }
    int mid = (tr[u].l + tr[u].r) >> 1;
    if (l <= mid) modify(u << 1, l, r, k);
    if (r >= mid + 1) modify(u << 1 | 1, l, r, k);
    pushup(u);
}
```

## 线段树分裂

> 板子题网址: https://www.luogu.com.cn/problem/P3372

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
