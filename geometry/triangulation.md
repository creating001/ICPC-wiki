# 三角剖分

> 板子题网址: https://vjudge.net/problem/POJ-3675

```cpp

inline int sign(double x) {
    if (fabs(x) < EPS) return 0;
    return x < 0 ? -1 : 1;
}

inline double len(PDD a) {
    return sqrt(a & a);
}

inline PDD unit(PDD a) {
    return {a.F / len(a), a.S / len(a)};
}

inline PDD rotate(PDD a, double b) {
    return {a.F * cos(b) + a.S * sin(b), -a.F * sin(b) + a.S * cos(b)};
}

inline PDD GLI(PDD a, PDD v, PDD b, PDD w) {
    auto u = a - b;
    auto t = (w * u) / (v * w);
    return a + v * t;
}

inline bool on_segment(PDD v, PDD a, PDD b) {
    return sign((a - v) * (b - v)) == 0 && sign((v - a) & (v - b)) <= 0;
}

inline double get_sector(PDD a, PDD b) {
    auto o = acos((a & b) / len(a) / len(b));
    if (sign(a * b) < 0) o = -o;
    return R * R * o / 2;
}

inline double get_circle_line_intersection(PDD a, PDD b, PDD& pa, PDD& pb) {
    auto e = GLI(a, b - a, {0, 0}, rotate(b - a, -PI / 2));
    auto d = len(e);
    if (!on_segment(e, a, b)) d = min(len(a), len(b));
    if (sign(R - len(e)) <= 0) return d;
    auto l = sqrt(R * R - len(e) * len(e));
    pa = e + unit(a - b) * l;
    pb = e + unit(b - a) * l;
    return d;
}

inline double get_circle_triangle_area(PDD a, PDD b) {
    double da = len(a), db = len(b);
    if (sign(a * b) == 0) return 0;
    if (sign(R - da) >= 0 && sign(R - db) >= 0) return a * b / 2;
    PDD pa, pb;
    double d = get_circle_line_intersection(a, b, pa, pb);
    if (sign(R - d) <= 0) return get_sector(a, b);
    if (sign(R - da) >= 0) return (a * pb) / 2 + get_sector(pb, b);
    if (sign(R - db) >= 0) return (pa * b) / 2 + get_sector(a, pa);
    return get_sector(a, pa) + (pa * pb) / 2 + get_sector(pb, b);
}
```
