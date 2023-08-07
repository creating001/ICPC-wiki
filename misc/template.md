# Trick

## 倍增长度

```cpp
template<int len = 1>
inline void solve(int n) {
    if (len < n) return solve<len * 2>(n);
    bitset<len> bs;
}
```
