# 树状数组

## 一维树状数组

> 板子题网址: https://www.luogu.com.cn/problem/P3374

```cpp
inline void init() {
    for (int i = 1; i <= n; i++)
        tr[i] = s[i] - s[i - lowbit(i)];
}

inline void add(int p, int c) {
    for (int i = p; i <= n; i += lowbit(i)) tr[i] += c;
}

inline int sum(int p) {
    int ans = 0;
    for (int i = p; i; i -= lowbit(i)) ans += tr[i];
    return ans;
}
```

## 二维树状数组

> 板子题网址: https://loj.ac/p/133

```cpp

```
