# DSU

## 普通 DSU

> 板子题网址: https://www.luogu.com.cn/problem/P3367

```cpp
struct DSU {
    vector<int> p;
    inline DSU(int n) : p(n) {
        for (int i = 0; i < n; i++) p[i] = i;
    }
    inline int find(int x) {
        return (p[x] == x ? x : p[x] = find(p[x]));
    }
    inline void unite(int x, int y) {
        p[find(x)] = find(y);
    }
} dsu(N);
```

## 带权 DSU

> 板子题网址: https://www.luogu.com.cn/problem/P2024

```cpp

```

## 扩展域 DSU

> 板子题网址: https://www.acwing.com/problem/content/241

```cpp

```

## 可持久化 DSU

> 板子题网址: https://www.luogu.com.cn/problem/P3402

```cpp

```
