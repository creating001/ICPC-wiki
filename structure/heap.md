# 二叉堆

> 板子题网址: https://www.luogu.com.cn/problem/P3378

## 数据存储

```cpp
int h[N], siz;
```

## 向下调整
```cpp
inline void down(int k) {
    int t = k;
    if (2 * k <= siz && h[2 * k] < h[t]) t = 2 * k;
    if (2 * k + 1 <= siz && h[2 * k + 1] < h[t]) t = 2 * k + 1;
    if (t != k) swap(h[t], h[k]), down(t);
}
```

## 向上调整
```cpp
inline void up(int k) {
    while (k / 2 && h[k / 2] > h[k])
        swap(h[k / 2], h[k]), k /= 2;
}
```

## 建堆
```cpp
inline void build(int size) {
    siz = size;
    for (int i = siz / 2; i; i--) down(i);
}
```

## 插入
```cpp
inline void insert(int x) {
    h[++siz] = x, up(siz);
}
```

## 删除
```cpp
inline void remove(int k) {
    swap(h[k], h[siz--]), down(k), up(k);
}
```

## 修改
```cpp
inline void modify(int k, int x) {
    h[k] = x, down(k), up(k);
}
```

## 查询
```cpp
inline int query(int k) {
    return h[k];
}
```
