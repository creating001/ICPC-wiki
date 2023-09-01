# 堆

## 二叉堆

> 板子题网址: https://www.luogu.com.cn/problem/P3378

```cpp
struct Heap {
    vector<int> h;
    inline Heap() : h(1) {}
    inline void up(int k) {
        while (k / 2 && h[k / 2] > h[k])
            swap(h[k / 2], h[k]), k /= 2;
    }
    inline void down(int k) {
        int t = k;
        if (2 * k < h.size() && h[2 * k] < h[t]) t = 2 * k;
        if (2 * k + 1 < h.size() && h[2 * k + 1] < h[t]) t = 2 * k + 1;
        if (t != k) swap(h[t], h[k]), down(t);
    }
    inline void insert(int x) {
        h.emplace_back(x), up(h.size() - 1);
    }
    inline void remove() {
        swap(h[1], h.back()), h.pop_back(), down(1);
    }
} heap;
```

## 可并堆(左偏树)

> 板子题网址: https://www.luogu.com.cn/problem/P3377

```cpp
inline int merge(int x, int y) {
    if (!x || !y) return x + y;
    if (val[x] > val[y]) swap(x, y);
    R[x] = merge(R[x], y);
    if (dis[R[x]] > dis[L[x]]) swap(L[x], R[x]);
    dis[x] = dis[L[x]] + 1;
    return x;
}
```
