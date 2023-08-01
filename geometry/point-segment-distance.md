# 点到线段距离

```cpp
inline double p_to_p(PII& p1, PII& p2) {
    return sqrt(double((p1.F - p2.F) * (p1.F - p2.F) + (p1.S - p2.S) * (p1.S - p2.S)));
}

inline double point_to_line(PII& p, PII& p1, PII& p2) {
    LL cross = (p2.F - p1.F) * (p.F - p1.F) + (p2.S - p1.S) * (p.S - p1.S);
    if (cross <= 0) return p_to_p(p, p1);

    LL d = (p2.F - p1.F) * (p2.F - p1.F) + (p2.S - p1.S) * (p2.S - p1.S);
    if (cross >= d) return p_to_p(p, p2);

    return fabs(double((p1.S - p2.S) * p.F - (p1.F - p2.F) * p.S + (p1.F * p2.S - p2.F * p1.S)) / p_to_p(p1, p2));
}
```
