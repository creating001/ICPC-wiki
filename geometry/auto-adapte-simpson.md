# 自适应辛普森积分

> 板子题网址: https://www.luogu.com.cn/problem/P4525
>
> 板子题网址: https://www.luogu.com.cn/problem/P4526

```cpp
inline double get(double l, double r) {
    return (r - l) * (f(l) + f(r) + 4 * f((l + r) / 2)) / 6;
}

inline double simpson(double l, double r, double s) {
    double lm = l + (r - l) / 3, rm = lm + (r - l) / 3;
    double ls = get(l, lm), ms = get(lm, rm), rs = get(rm, r);
    if (fabs(ls + rs + ms - s) < EPS) return ls + rs + ms;
    return simpson(l, lm, ls) + simpson(lm, rm, ms) + simpson(rm, r, rs);
}
```
