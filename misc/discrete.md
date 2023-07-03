# 离散化处理

## 整数离散化

> 板子题网址: https://www.luogu.com.cn/problem/U183572

时间复杂度: $O(N \times \log N )$

```cpp
vector<int> a, b; // b 是 a 的一个副本
sort(a.begin(), a.end());
a.erase(unique(a.begin(), a.end()), a.end());
for (int i = 0; i < n; ++i)
    b[i] = lower_bound(a.begin(), a.end(), b[i]) - a.begin();
```
