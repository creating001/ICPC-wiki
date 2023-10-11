# manacher

> 板子题网址: https://www.luogu.com.cn/problem/P3805

```cpp
inline void change() {
    b[m++] = '$', b[m++] = '#';
    for (int i = 0; i < n; i++) b[m++] = a[i], b[m++] = '#';
    b[m++] = '^';
}

inline void manacher() {
    int r = 0, mid = 0;
    for (int i = 0; i < m; i++) {
        if (i < r) p[i] = min(p[2 * mid - i], r - i);
        else p[i] = 1;
        while (b[i - p[i]] == b[i + p[i]]) p[i]++;
        if (i + p[i] > r) r = i + p[i], mid = i;
    }
}
```
