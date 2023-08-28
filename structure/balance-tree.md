# 平衡树

## Treap

> 板子题网址: https://www.luogu.com.cn/problem/P6136

```cpp
inline int get_node(int key) {
    tr[++idx].key = key;
    tr[idx].val = rand();
    tr[idx].cnt = tr[idx].siz = 1;
    return idx;
}

inline void pushup(int p) {
    tr[p].siz = tr[tr[p].l].siz + tr[tr[p].r].siz + tr[p].cnt;
}

inline void zig(int& p) {
    int q = tr[p].l;
    tr[p].l = tr[q].r, tr[q].r = p, p = q;
    pushup(tr[p].r), pushup(p);
}

inline void zag(int& p) {
    int q = tr[p].r;
    tr[p].r = tr[q].l, tr[q].l = p, p = q;
    pushup(tr[p].l), pushup(p);
}

inline void build() {
    get_node(-INF), get_node(INF);
    root = 1, tr[1].r = 2;
    pushup(root);
    if (tr[root].val < tr[2].val) zag(root);
}

inline void insert(int& p, int key) {
    if (!p) p = get_node(key);
    else if (tr[p].key == key) tr[p].cnt++;
    else if (tr[p].key > key) {
        insert(tr[p].l, key);
        if (tr[tr[p].l].val > tr[p].val) zig(p);
    } else {
        insert(tr[p].r, key);
        if (tr[tr[p].r].val > tr[p].val) zag(p);
    }
    pushup(p);
}

inline void remove(int& p, int key) {
    if (!p) return;
    if (tr[p].key == key) {
        if (tr[p].cnt > 1) tr[p].cnt--;
        else if (tr[p].l || tr[p].r) {
            if (!tr[p].r || tr[tr[p].l].val > tr[tr[p].r].val) {
                zig(p);
                remove(tr[p].r, key);
            } else {
                zag(p);
                remove(tr[p].l, key);
            }
        } else p = 0;
    } else if (tr[p].key > key) remove(tr[p].l, key);
    else remove(tr[p].r, key);
    pushup(p);
}

inline int get_rank(int p, int key) {
    if (!p) return 1;
    if (tr[p].key == key) return tr[tr[p].l].siz + 1;
    if (tr[p].key > key) return get_rank(tr[p].l, key);
    return tr[tr[p].l].siz + tr[p].cnt + get_rank(tr[p].r, key);
}

inline int get_key(int p, int rank) {
    if (!p) return INF;
    if (tr[tr[p].l].siz >= rank) return get_key(tr[p].l, rank);
    if (tr[tr[p].l].siz + tr[p].cnt >= rank) return tr[p].key;
    return get_key(tr[p].r, rank - tr[tr[p].l].siz - tr[p].cnt);
}

inline int get_pre(int p, int key) {
    if (!p) return -INF;
    if (tr[p].key >= key) return get_pre(tr[p].l, key);
    return max(tr[p].key, get_pre(tr[p].r, key));
}

inline int get_nex(int p, int key) {
    if (!p) return INF;
    if (tr[p].key <= key) return get_nex(tr[p].r, key);
    return min(tr[p].key, get_nex(tr[p].l, key));
}
```

## Splay

> 板子题网址: https://www.luogu.com.cn/problem/P6136

```cpp
struct Splay {
    int s[2], p, v;
    int size, cnt;
} tr[N];
int root, idx;

inline void pushup(int u) {
    tr[u].size = tr[tr[u].s[0]].size + tr[tr[u].s[1]].size + tr[u].cnt;
}

inline void rotate(int u) {
    int y = tr[u].p, z = tr[y].p;
    int k = u == tr[y].s[1];
    tr[z].s[tr[z].s[1] == y] = u, tr[u].p = z;
    tr[y].s[k] = tr[u].s[k ^ 1], tr[tr[u].s[k ^ 1]].p = y;
    tr[u].s[k ^ 1] = y, tr[y].p = u;
    pushup(y), pushup(u);
}

inline void splay(int u, int v) {
    while (tr[u].p != v) {
        int y = tr[u].p, z = tr[y].p;
        if (z != v) {
            if ((y == tr[z].s[1]) != (u == tr[y].s[1])) rotate(u);
            else rotate(y);
        }
        rotate(u);
    }
    if (!v) root = u;
}

inline void insert(int x) {
    int u = root, p = 0;
    while (u) {
        if (tr[u].v == x) return splay(u, 0), tr[u].cnt++, void();
        p = u;
        u = tr[u].s[x > tr[u].v];
    }
    u = ++idx;
    if (p) tr[p].s[x > tr[p].v] = u;
    tr[u].p = p, tr[u].v = x, tr[u].cnt = 1;
    splay(u, 0);
}

inline void find(int x) {
    int u = root;
    while (tr[u].v != x && tr[u].s[x > tr[u].v])
        u = tr[u].s[x > tr[u].v];
    splay(u, 0);
}

inline int get_pre(int x) {
    find(x);
    if (tr[root].v < x) return root;
    int u = tr[root].s[0];
    while (tr[u].s[1]) u = tr[u].s[1];
    splay(u, 0);
    return u;
}

inline int get_nex(int x) {
    find(x);
    if (tr[root].v > x) return root;
    int u = tr[root].s[1];
    while (tr[u].s[0]) u = tr[u].s[0];
    splay(u, 0);
    return u;
}

inline int get_rank(int x) {
    find(x);
    if (tr[root].v >= x) return tr[tr[root].s[0]].size;
    return tr[tr[root].s[0]].size + tr[root].cnt;
}

inline int get_k(int k) {
    int u = root;
    while (true) {
        if (tr[tr[u].s[0]].size >= k) u = tr[u].s[0];
        else if (tr[tr[u].s[0]].size + tr[u].cnt >= k) return splay(u, 0), u;
        else k -= tr[tr[u].s[0]].size + tr[u].cnt, u = tr[u].s[1];
    }
}

inline void remove(int x) {
    int l = get_pre(x), r = get_nex(x);
    splay(l, 0);
    splay(r, l);
    int u = tr[r].s[0];
    if (tr[u].cnt > 1) {
        tr[u].cnt--;
        splay(u, 0);
        return;
    }
    tr[r].s[0] = 0;
}
```

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
