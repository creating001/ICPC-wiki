# 最小圆覆盖

> 板子题网址: https://www.luogu.com.cn/problem/P1742

```cpp
inline PDD operator-(PDD a, PDD b) {
    return {a.F - b.F, a.S - b.S};
}

inline PDD operator+(PDD a, PDD b) {
    return {a.F + b.F, a.S + b.S};
}

inline double operator*(PDD a, PDD b) {
    return a.F * b.S - a.S * b.F;
}

inline PDD operator*(PDD a, double t) {
    return {a.F * t, a.S * t};
}

inline PDD operator/(PDD a, double t) {
    return {a.F / t, a.S / t};
}

inline int sign(double x) {
    if (fabs(x) < EPS) return 0;
    return x < 0 ? -1 : 1;
}

inline double dis(PDD a, PDD b) {
    double dx = a.F - b.F;
    double dy = a.S - b.S;
    return sqrt(dx * dx + dy * dy);
}

inline PDD GLI(PDD a, PDD v, PDD b, PDD w) {
    auto u = a - b;
    auto t = (w * u) / (v * w);
    return a + v * t;
}

inline PDD rotate(PDD a, double b) {
    return {a.F * cos(b) + a.S * sin(b), -a.F * sin(b) + a.S * cos(b)};
}

inline Circle get(PDD a, PDD b, PDD c) {
    PPP l1 = {(a + b) / 2, rotate(b - a, PI / 2)};
    PPP l2 = {(a + c) / 2, rotate(c - a, PI / 2)};
    auto center = GLI(l1.F, l1.S, l2.F, l2.S);
    return {center, dis(center, a)};
}

inline bool in_circle(Circle a, PDD b) {
    return sign(a.S - dis(b, a.F)) >= 0;
}

inline Circle MCC() {
    Circle ans = {p[0], 0};
    for (int i = 1; i < n; i++) {
        if (in_circle(ans, p[i])) continue;
        ans = {p[i], 0};
        for (int j = 0; j < i; j++) {
            if (in_circle(ans, p[j])) continue;
            ans = {(p[i] + p[j]) / 2, dis(p[i], p[j]) / 2};
            for (int k = 0; k < j; k++) {
                if (in_circle(ans, p[k])) continue;
                ans = get(p[i], p[k], p[j]);
            }
        }
    }
    return ans;
}
```
