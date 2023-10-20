# 字典树

## Trie

> 板子题网址: https://www.luogu.com.cn/problem/P8306

```cpp
inline void insert(const char* str) {
    for (int i = 0, p = 0; str[i]; i++) {
        int u = str[i] - 'a';
        if (!tr[p][u]) tr[p][u] = ++idx;
        p = tr[p][u], cnts[p]++;
    }
}

inline int query(const char* str) {
    int p = 0;
    for (int i = 0; str[i]; i++) {
        int u = str[i] - 'a';
        if (!tr[p][u]) return 0;
        p = tr[p][u];
    }
    return cnts[p];
}
```

## O1 Trie

> 板子题网址: https://www.luogu.com.cn/problem/P4551

```cpp

```

## Tire 合并

> 板子题网址: https://www.luogu.com.cn/problem/P6623

```cpp

```

## 可持久化 Trie

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

## 最小异或树

> 板子题网址: https://codeforces.com/problemset/problem/888/G

```cpp

```
