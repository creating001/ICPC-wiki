# 树状数组

> 板子题网址: https://www.luogu.com.cn/problem/P3374
>
> 板子题网址: https://www.luogu.com.cn/problem/P3368

## $lowbit$ 操作

```cpp
inline int lowbit(int x) {
    return x & -x;
}
```

## $O(n)$ 初始化

```cpp
for (int i = 1; i <= n; i++) tr[i] = s[i] - s[i - lowbit(i)];
```

## 单点修改

```cpp
inline void add(int p, int c) {
    for (int i = p; i <= n; i += lowbit(i)) tr[i] += c;
}
```

## 区间查询

```cpp
inline int sum(int p) {
    int ans = 0;
    for (int i = p; i; i -= lowbit(i)) ans += tr[i];
    return ans;
}
```
