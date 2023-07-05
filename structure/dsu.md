# 并查集

> 板子题网址: https://www.luogu.com.cn/problem/P3375

## 初始化

```cpp
inline void init() {
    for (int i = 0; i < N; i++) p[i] = i;
}
```

## 查找

```cpp
inline int find(int x) {
    return (p[x] == x ? x : p[x] = find(p[x]));
}
```

## 合并

```cpp
inline void unite(int x, int y) {
    p[find(x)] = find(y);
}
```

## 判断是否连通

```cpp
inline bool is_connect(int x, int y) {
    return find(x) == find(y);
}
```
