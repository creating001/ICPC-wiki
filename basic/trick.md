# Trick

## 倍增长度

```cpp
template<int len = 1>
inline void solve(int n) {
    if (len < n) return solve<max(len * 2, N)>(n);
    bitset<len> f;
}
```

## 随机数生成

```cpp
static random_device dev;
static mt19937 eng(dev());
static uniform_int_distribution<int> u(0, INT_MAX);
```
