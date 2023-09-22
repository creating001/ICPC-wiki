# 自适应辛普森积分

> 板子题网址: https://www.luogu.com.cn/problem/P4525
>
> 板子题网址: https://www.luogu.com.cn/problem/P4526

```cpp
inline double get(double l, double r) {
    return (r - l) * (f(l) + f(r) + 4 * f((l + r) / 2)) / 6;
}

inline double simpson(double l, double r) {
    double ans = get(l, r);
    double mid = (l + r) / 2;
    double nex = get(l, mid) + get(mid, r);
    if (fabs(ans - nex) < EPS) return ans;
    return simpson(l, mid) + simpson(mid, r);
}
```
