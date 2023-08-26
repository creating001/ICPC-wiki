# 字符串匹配

## KMP匹配

> 板子题网址: https://www.luogu.com.cn/problem/P3375

时间复杂度: $O(N + M)$

```cpp
char p[N], s[M];
int n, m, nex[N];

for (int i = 2, j = 0; i <= n; i++) {
    while (j && p[j + 1] != p[i]) j = nex[j];
    if (p[j + 1] == p[i]) j++;
    nex[i] = j;
}

for (int i = 1, j = 0; i <= m; i++) {
    while (j && p[j + 1] != s[i]) j = nex[j];
    if (p[j + 1] == s[i]) j++;
    if (j == n) {
        j = nex[j];
    }
}
```

## 字符串哈希

> 板子题网址: https://www.luogu.com.cn/problem/P3370

```cpp
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

## 扩展 KMP(Z 函数)

> 板子题网址: https://www.luogu.com.cn/problem/P5410

```cpp

```
