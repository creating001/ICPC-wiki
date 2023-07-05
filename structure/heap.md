# 二叉堆

## 普通二叉堆

> 板子题网址: https://www.luogu.com.cn/problem/P3378

```cpp
// h是堆, siz是堆的大小
int h[N], siz;

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

for (int i = siz / 2; i; i--) down(i);

inline void insert(int x) {
    h[++siz] = x, up(siz);
}

inline void remove() {
    swap(h[1], h[siz--]), down(1);
}
```

## 维护信息更多的二叉堆

> 板子题网址: https://www.acwing.com/problem/content/841

```cpp
// h是堆, ph是第k个插入的点在堆中的位置, hp是堆中位置k的点是第几个插入的
// idx是插入点的编号, siz是堆的大小
int h[N], hp[N], ph[N], siz, idx;

inline void h_swap(int k1, int k2) {
    swap(h[k1], h[k2]);
    swap(ph[hp[k1]], ph[hp[k2]]);
    swap(hp[k1], hp[k2]);
}

inline void down(int k) {
    int t = k;
    if (2 * k <= siz && h[t] > h[2 * k]) t = 2 * k;
    if (2 * k + 1 <= siz && h[t] > h[2 * k + 1]) t = 2 * k + 1;
    if (t != k) h_swap(t, k), down(t);
}

inline void up(int k) {
    while (k / 2 && h[k / 2] > h[k])
        h_swap(k / 2, k), k /= 2;
}

inline void remove(int i_k) {
    int pos = ph[i_k];
    h_swap(pos, siz--), down(pos), up(pos);
}

inline int query(int i_k) {
    return h[ph[i_k]];
}

inline void modify(int i_k, int x) {
    h[ph[i_k]] = x, down(ph[i_k]), up(ph[i_k]);
}

inline void insert(int x) {
    siz++, idx++;
    h[siz] = x, ph[idx] = siz, hp[siz] = idx;
    up(siz);
}
```
