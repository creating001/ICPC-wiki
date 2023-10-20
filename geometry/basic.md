# 计算几何基础

## 二维计算几何基础

```cpp
using PDD = pair<double, double>;

inline PDD operator-(PDD a, PDD b) {
    return {a.F - b.F, a.S - b.S};
}

inline double operator*(PDD a, PDD b) {
    return a.F * b.S - a.S * b.F;
}

inline PDD rotate(PDD a, double b) {
    return {a.F * cos(b) + a.S * sin(b), -a.F * sin(b) + a.S * cos(b)};
}

inline double get_angle(PDD a, PDD b) {
    return atan2(b.S - a.S, b.F - a.F);
}

inline PDD GLI(PDD a, PDD v, PDD b, PDD w) {
    auto u = a - b;
    auto t = (w * u) / (v * w);
    return {a.F + v.F * t, a.S + v.S * t};
}
```

## 三维计算几何基础

```cpp

```
