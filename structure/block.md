# 分块

## 分块初步

> 板子题网址: https://www.luogu.com.cn/problem/P3372

```cpp
struct Block {
    LL add, sum;
} b[M];
int a[N], s[N], len;

inline void modify(int l, int r, int c) {
    if (s[l] == s[r]) {
        for (int i = l; i <= r; i++)
            a[i] += c, b[s[i]].sum += c;
    } else {
        int i = l, j = r;
        while (s[i] == s[l]) a[i] += c, b[s[i]].sum += c, i++;
        while (s[j] == s[r]) a[j] += c, b[s[j]].sum += c, j--;
        for (int k = s[i]; k <= s[j]; k++)
            b[k].sum += len * c, b[k].add += c;
    }
}

inline LL query(int l, int r) {
    if (s[l] == s[r]) {
        LL ans = 0;
        for (int i = l; i <= r; i++) ans += a[i] + b[s[i]].add;
        return ans;
    } else {
        LL ans = 0;
        int i = l, j = r;
        while (s[i] == s[l]) ans += a[i] + b[s[i]].add, i++;
        while (s[j] == s[r]) ans += a[j] + b[s[j]].add, j--;
        for (int k = s[i]; k <= s[j]; k++) ans += b[k].sum;
        return ans;
    }
}
```
