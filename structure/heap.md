# 二叉堆

## 普通二叉堆

> 板子题网址: https://www.luogu.com.cn/problem/P3378

```cpp
inline void down(int k) {
    int t = k;
    if (2 * k <= siz && h[2 * k] < h[t]) t = 2 * k;
    if (2 * k + 1 <= siz && h[2 * k + 1] < h[t]) t = 2 * k + 1;
    if (t != k) swap(h[t], h[k]), down(t);
}

inline void up(int k) {
    while (k / 2 && h[k / 2] > h[k])
        swap(h[k / 2], h[k]), k /= 2;
}
```

## 维护信息更多的二叉堆

> 板子题网址: https://www.acwing.com/problem/content/841

```cpp
inline void heap_swap(int k1, int k2) {
    swap(h[k1], h[k2]);
    swap(ph[hp[k1]], ph[hp[k2]]);
    swap(hp[k1], hp[k2]);
}

inline void down(int k) {
    int t = k;
    if (2 * k <= siz && h[2 * k] < h[t]) t = 2 * k;
    if (2 * k + 1 <= siz && h[2 * k + 1] < h[t]) t = 2 * k + 1;
    if (t != k) heap_swap(t, k), down(t);
}

inline void up(int k) {
    while (k / 2 && h[k / 2] > h[k])
        heap_swap(k, k / 2), k /= 2;
}

inline void insert(int x) {
    siz++, idx++;
    h[siz] = x, ph[idx] = siz, hp[siz] = idx;
    up(siz);
}

inline void modify(int k_i, int x) {
    int k = ph[k_i];
    h[k] = x;
    down(k), up(k);
}

inline void remove(int k_i) {
    int k = ph[k_i];
    heap_swap(siz--, k);
    down(k), up(k);
}
```
