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

> 板子题网址: https://www.luogu.com.cn/problem/P3391

```cpp

```
