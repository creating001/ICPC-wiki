# 线性DP

## 最长上升子序列

给定一个长度为 $n$ 的序列 $a$，求它的最长上升子序列的长度。

```cpp
inline int LIS(int* a, int n) {
    for (int i = 0; i < n; i++) dp[i] = 1;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < i; j++)
            if (a[i] > a[j])
                dp[i] = max(dp[i], dp[j] + 1);
    return *max_element(dp, dp + n);
}
```

```cpp

```
