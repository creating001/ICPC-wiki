# 二分查找

时间复杂度: $O(\log N)$

## 整数二分

> 板子题网址1: https://www.acwing.com/problem/content/791
>
> 板子题网址2: https://www.luogu.com.cn/problem/P2249

```cpp
bool check(int x); // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用:
inline int bsearch_1(int l, int r) {
    while (l < r) {
        int mid = (l + r) >> 1;
        if (check(mid)) r = mid;
        else l = mid + 1;
    }
    return l;
}

// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用:
inline int bsearch_2(int l, int r) {
    while (l < r) {
        int mid = (l + r + 1) >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

## 浮点数二分

> 板子题网址1: https://www.acwing.com/problem/content/description/792

```cpp
inline double bsearch(double l, double r) {
    while (r - l > ESP) {
        double mid = (r + l) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
}
```
