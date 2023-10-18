# 半平面交

> 板子题网址: https://www.luogu.com.cn/problem/P4196

```cpp
inline PDD operator-(PDD a, PDD b) {
    return {a.F - b.F, a.S - b.S};
}

inline double operator*(PDD a, PDD b) {
    return a.F * b.S - a.S * b.F;
}

inline int sign(double x) {
    if (fabs(x) < EPS) return 0;
    return x < 0 ? -1 : 1;
}

inline double get_angle(Line a) {
    return atan2(a.t.S - a.s.S, a.t.F - a.s.F);
}

inline bool cmp(Line a, Line b) {
    auto A = get_angle(a), B = get_angle(b);
    if (sign(A - B) == 0) return ((a.t - b.s) * (b.t - b.s)) < 0;
    return A < B;
}

inline PDD GLI(PDD p, PDD v, PDD q, PDD w) {
    auto u = p - q;
    auto t = (w * u) / (v * w);
    return {p.F + t * v.F, p.S + v.S * t};
}

inline PDD GLI(Line a, Line b) {
    return GLI(a.s, a.t - a.s, b.s, b.t - b.s);
}

inline bool on_right(Line a, Line b, Line c) {
    auto p = GLI(b, c);
    return ((a.t - a.s) * (p - a.s)) < 0;
}

inline void HPI() {
    sort(line, line + cnts, cmp);
    int hh = 0, tt = -1;
    for (int i = 0; i < cnts; i++) {
        if (i && sign(get_angle(line[i]) - get_angle(line[i - 1])) == 0) continue;
        while (hh + 1 <= tt && on_right(line[i], line[que[tt - 1]], line[que[tt]]))
            tt--;
        while (hh + 1 <= tt && on_right(line[i], line[que[hh]], line[que[hh + 1]]))
            hh++;
        que[++tt] = i;
    }
    while (hh + 1 <= tt && on_right(line[hh], line[que[tt - 1]], line[que[tt]]))
        tt--;
    while (hh + 1 <= tt && on_right(line[tt], line[que[hh]], line[que[hh + 1]]))
        hh++;
}
```
