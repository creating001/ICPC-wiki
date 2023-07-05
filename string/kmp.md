# KMP匹配算法

> 板子题网址: https://www.luogu.com.cn/problem/P3375

时间复杂度: $O(N + M)$

```cpp
char p[N], s[M];
int n, m, nex[N];

// 构建next数组
for (int i = 2, j = 0; i <= n; i++) {
    while (j && p[j + 1] != p[i]) j = nex[j];
    if (p[j + 1] == p[i]) j++;
    nex[i] = j;
}

// KMP匹配
for (int i = 1, j = 0; i <= m; i++) {
    while (j && p[j + 1] != s[i]) j = nex[j];
    if (p[j + 1] == s[i]) j++;
    if (j == n) {
        j = nex[j];
    }
}
```
