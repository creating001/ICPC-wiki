# 计算机几何基础

```cpp
using PII = pair<int, int>;

inline PII operator-(PII a, PII b) {
    return {a.F - b.F, a.S - b.S};
}

inline PII operator+(PII a, PII b) {
    return {a.F + b.F, a.S + b.S};
}

inline int operator*(PII a, PII b) {
    return a.F * b.S + a.S * b.F;
}

inline int operator^(PII a, PII b) {
    return a.F * b.S - a.S * b.F;
}
```
