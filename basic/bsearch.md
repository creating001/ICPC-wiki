# 二分查找

## 整数二分

> 板子题网址: https://www.acwing.com/problem/content/791

```cpp
inline int bsearch_1(int l, int r) {
    while (l < r) {
        int mid = (l + r) >> 1;
        if (check(mid)) r = mid;
        else l = mid + 1;
    }
    return l;
}

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

> 板子题网址: https://www.acwing.com/problem/content/description/792

```cpp
inline double bsearch(double l, double r) {
    while (r - l > ESP) {
        double mid = (r + l) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
}
```

## 三分法

> 板子题网址: https://www.luogu.com.cn/problem/P3382

```cpp
while (r - l > EPS) {
    double lm = l + (r - l) / 3;
    double rm = l + (r - l) / 3 * 2;
    if (f(lm) > f(rm)) r = rm;
    else l = lm;
}
```
