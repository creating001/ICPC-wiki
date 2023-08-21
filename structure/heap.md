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
