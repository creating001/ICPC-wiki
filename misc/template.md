# Trick

## 倍增长度

```cpp
template<int len = 1>
inline void solve(int n) {
    if (len < n) return solve<max(len * 2, N)>(n);
    bitset<len> f;
}
```
