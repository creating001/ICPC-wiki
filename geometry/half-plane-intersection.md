# 半平面交

> 板子题网址: https://www.luogu.com.cn/problem/P4196

```cpp
inline double get_angle(Line x) {
    return atan2(x.t.S - x.s.S, x.t.F - x.s.F);
}

inline double cross(PDD a, PDD b) {
    return a.F * b.S - a.S * b.F;
}

inline bool cmp(Line a, Line b) {
    auto A = get_angle(a), B = get_angle(b);
    if (sign(A - B) == 0) return cross(b.t - b.s, a.t - b.s) > 0;
    return A < B;
}

inline PDD GLI(PDD p, PDD v, PDD q, PDD w) {
    auto u = p - q;
    auto t = cross(w, u) / cross(v, w);
    return {p.F + t * v.F, p.S + t * v.S};
}

inline PDD GLI(Line a, Line b) {
    return GLI(a.s, a.t - a.s, b.s, b.t - b.s);
}

inline bool on_right(Line a, Line b, Line c) {
    auto p = GLI(b, c);
    return sign(cross(a.t - a.s, p - a.s)) <= 0;
}

inline double HPI() {
    sort(l, l + cnts, cmp);
    int hh = 0, tt = -1;
    for (int i = 0; i < cnts; i++) {
        if (i && sign(get_angle(l[i]) - get_angle(l[i - 1])) == 0) continue;
        while (hh + 1 <= tt && on_right(l[i], l[que[tt - 1]], l[que[tt]])) tt--;
        while (hh + 1 <= tt && on_right(l[i], l[que[hh]], l[que[hh + 1]])) hh++;
        que[++tt] = i;
    }
    while (hh + 1 <= tt && on_right(l[que[hh]], l[que[tt - 1]], l[que[tt]])) tt--;
    while (hh + 1 <= tt && on_right(l[que[tt]], l[que[hh]], l[que[hh + 1]])) hh++;
    que[++tt] = que[hh];
    int idx = 0;
    for (int i = hh; i < tt; i++) {
        ans[idx++] = GLI(l[que[i]], l[que[i + 1]]);
    }
    double res = 0;
    for (int i = 1; i + 1 < idx; i++) {
        res += cross(ans[i] - ans[0], ans[i + 1] - ans[0]);
    }
    return res / 2;
}
```
