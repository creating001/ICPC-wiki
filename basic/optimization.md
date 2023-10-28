# 常见优化技巧

## 二分

## 双指针

## 单调栈

> 板子题网址: https://www.luogu.com.cn/problem/P5788

```cpp
vector<int> stk;
for (int i = n; i >= 1; i--) {
    while (!stk.empty() && a[stk.back()] <= a[i]) stk.pop_back();
    if (!stk.empty()) f[i] = stk.back();
    stk.push_back(i);
}
```

## 单调队列

> 板子题网址: https://www.luogu.com.cn/problem/P1886
> 板子题网址: https://www.luogu.com.cn/problem/P2216

```cpp
int hh = 0, tt = -1;
for (int i = 1; i <= n; i++) {
    while (hh <= tt && q[hh] <= i - k) hh++;
    while (hh <= tt && a[q[tt]] >= a[i]) tt--;
    q[++tt] = i;
    if (i >= k) mn[i - k + 1] = a[q[hh]];
}
hh = 0, tt = -1;
for (int i = 1; i <= n; i++) {
    while (hh <= tt && q[hh] <= i - k) hh++;
    while (hh <= tt && a[q[tt]] <= a[i]) tt--;
    q[++tt] = i;
    if (i >= k) mx[i - k + 1] = a[q[hh]];
}
```
