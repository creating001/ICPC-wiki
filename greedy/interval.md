# 区间问题

## 区间选点

> 板子题网址: https://www.acwing.com/problem/content/907

```cpp
inline int solve(PII* a, int n) {
    sort(a, a + n, [](PII& a, PII& b) -> bool {
        if (a.S != b.S) return a.S < b.S;
        return a.F < b.F;
    });
    int ans = 0;
    for (int i = 0; i < n; ans++) {
        int r = a[i].S;
        while (i < n && a[i].F <= r) i++;
    }
    return ans;
}
```

## 最大不相交区间数量

> 板子题网址: https://www.acwing.com/problem/content/910

```cpp
inline int solve(PII* a, int n) {
    sort(a, a + n, [](PII& a, PII& b) -> bool {
        if (a.R != b.R) return a.R < b.R;
        return a.L < b.L;
    });
    int ans = 0, r = -1e9 - 7;
    for (int i = 0; i < n; i++)
        if (r < a[i].L) ans++, r = a[i].R;
    return ans;
}
```

## 区间分组

> 板子题网址: https://www.acwing.com/problem/content/908

```cpp

```

## 区间覆盖

> 板子题网址: https://www.acwing.com/problem/content/909

```cpp

```
