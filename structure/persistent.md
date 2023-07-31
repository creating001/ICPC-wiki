# 可持久化数据结构

## 可持久化Trie树

> 板子题网址: https://www.luogu.com.cn/problem/P4735

```cpp
inline void insert(int k, int x, int p, int q) {
    id[q] = k;
    for (int i = 24; i >= 0; i--) {
        int u = x >> i & 1;
        if (tr[p][u ^ 1]) tr[q][u ^ 1] = tr[p][u ^ 1];
        tr[q][u] = ++idx;
        id[tr[q][u]] = k;
        p = tr[p][u], q = tr[q][u];
    }
}

inline int query(int p, int l, int x) {
    for (int i = 24; i >= 0; i--) {
        int u = x >> i & 1;
        if (id[tr[p][u ^ 1]] >= l) p = tr[p][u ^ 1];
        else p = tr[p][u];
    }
    return s[id[p]] ^ x;
}
```

## 可持久化数组

> 板子题网址: https://www.luogu.com.cn/problem/P3919

```cpp

```

## 可持久化线段树(主席树)

> 板子题网址: https://www.luogu.com.cn/problem/P3834

```cpp

```

## 可持久化并查集

> 板子题网址: https://www.luogu.com.cn/problem/P3402

```cpp

```

## 可持久化平衡树

> 板子题网址: https://www.luogu.com.cn/problem/P3835

```cpp

```

## 可持久化文艺平衡树

> 板子题网址: https://www.luogu.com.cn/problem/P5055

```cpp

```
