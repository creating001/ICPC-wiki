# 数位DP

> 板子题网址: https://www.luogu.com.cn/problem/P2602

```cpp
inline int get(vector<int>& a, int r, int l) {
    int ans = 0;
    for (int i = r; i >= l; i--)
        ans = ans * 10 + a[i];
    return ans;
}

inline int pow10(int k) {
    int ans = 1;
    while (k--) ans *= 10;
    return ans;
}

inline int count(int n, int x) {
    vector<int> a;
    while (n) a.emplace_back(n % 10), n /= 10;
    n = (int) a.size();
    int ans = 0;
    for (int i = n - 1 - !x; i >= 0; i--) {
        if (i < n - 1)
            ans += (get(a, n - 1, i + 1) - !x) * pow10(i);
        if (a[i] == x)
            ans += get(a, i - 1, 0) + 1;
        else if (a[i] > x)
            ans += pow10(i);
    }
    return ans;
}
```
