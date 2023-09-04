# 树状数组

## 一维树状数组

> 板子题网址: https://www.luogu.com.cn/problem/P3374

```cpp
struct BIT {
    vector<int> tr;
    inline BIT(int n) : tr(n) {}
    inline void add(int p, int c) {
        for (int i = p; i < tr.size(); i += i & -i)
            tr[i] += c;
    }
    inline int sum(int p) {
        int sum = 0;
        for (int i = p; i; i -= i & -i) sum += tr[i];
        return sum;
    }
    inline int findKth(int k) {
        int rank = 0, ans = 0;
        for (int i = 20; i >= 0; i--)
            if (ans + (1 << i) < tr.size() && rank + tr[ans + (1 << i)] < k)
                ans += (1 << i), rank += tr[ans];
        return ans + 1;
    }
} bit(N);
```

## 二维树状数组

> 板子题网址: https://loj.ac/p/133

```cpp

```
