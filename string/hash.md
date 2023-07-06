# 字符串哈希

> 板子题网址: https://www.luogu.com.cn/problem/P3370

```cpp
typedef unsigned long long ULL;
const int N = 1e5 + 5, P = 131;

char str[N];
ULL h[N], p[N];

inline void init() {
    p[0] = 1;
    for (int i = 1; str[i]; i++) {
        p[i] = p[i - 1] * P;
        h[i] = h[i - 1] * P + str[i];
    }
}

inline ULL get(int l, int r) {
    return h[r] - h[l - 1] * p[r - l + 1];
}
```
