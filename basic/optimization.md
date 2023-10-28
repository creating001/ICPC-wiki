# 常见优化技巧

## 二分

> 板子题网址: https://www.acwing.com/problem/content/104

## 倍增

> 板子题网址: https://www.luogu.com.cn/problem/P3865

```cpp
struct ST {
    static const int S = 20;
    vector<array<int, S>> dp;

    inline void init(const int* a, int n) {
        dp.resize(n + 1);
        for (int i = 1; i <= n; i++) dp[i][0] = a[i];
        for (int j = 1; j < S; j++)
            for (int i = 1; i + (1 << j) - 1 <= n; i++)
                dp[i][j] = gcd(dp[i][j - 1], dp[i + (1 << (j - 1))][j - 1]);
    }

    inline int query(int l, int r) {
        int k = log2(r - l + 1);
        return gcd(dp[l][k], dp[r - (1 << k) + 1][k]);
    }
} st;
```

## 双指针

> 板子题网址: https://www.acwing.com/problem/content/104

## 单调栈

> 板子题网址: https://www.luogu.com.cn/problem/P5788

## 单调队列

> 板子题网址: https://www.luogu.com.cn/problem/P1886
> 板子题网址: https://www.luogu.com.cn/problem/P2216
