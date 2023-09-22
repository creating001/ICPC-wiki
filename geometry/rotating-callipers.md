# 旋转卡壳

> 板子题网址: https://www.luogu.com.cn/problem/P1452

```cpp
inline PII operator-(PII a, PII b) {
    return {a.F - b.F, a.S - b.S};
}

inline int operator*(PII a, PII b) {
    return a.F * b.S - a.S * b.F;
}

inline int dis(PII a, PII b) {
    int dx = a.F - b.F;
    int dy = a.S - b.S;
    return dx * dx + dy * dy;
}

inline void andrew() {
    sort(p, p + n);
    for (int i = 0; i < n; i++) {
        while (top >= 2 && (p[i] - stk[top - 2]) * (stk[top - 1] - stk[top - 2]) >= 0) top--;
        stk[top++] = p[i];
    }
    int m = top;
    for (int i = n - 2; i >= 0; i--) {
        while (top >= m + 1 && (p[i] - stk[top - 2]) * (stk[top - 1] - stk[top - 2]) >= 0) top--;
        stk[top++] = p[i];
    }
    top--;
}

inline int rotating_calipers() {
    andrew();
    if (top <= 2) return dis(p[0], p[n - 1]);
    int ans = 0;
    for (int i = 0, j = 2; i < top; i++) {
        auto a = stk[i], b = stk[i + 1];
        while ((b - a) * (stk[j] - a) < (b - a) * (stk[j + 1] - a)) j = (j + 1) % top;
        ans = max(ans, max(dis(stk[j], a), dis(stk[j], b)));
    }
    return ans;
}
```
