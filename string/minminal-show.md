# 最小表示法

板子题网址: https://www.luogu.com.cn/problem/P1368

```cpp
inline string min_show(const string& s) {
    int i = 0, j = 1;
    while (i < n && j < n) {
        int k = 0;
        while (k < n && s[i + k] == s[j + k]) k++;
        if (k == n) break;
        if (s[i + k] > s[j + k]) i += k + 1;
        else j += k + 1;
        if (i == j) j++;
    }
    return s.substr(min(i, j), n);
}
```
