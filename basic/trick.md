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

## 快读快写

```cpp
struct IO {
    template<typename T = int>
    inline T read() {
        T x = 0;
        signed f = 1, ch = getchar();
        while (!isdigit(ch)) f = ch == '-' ? -1 : 1, ch = getchar();
        while (isdigit(ch)) x = x * 10 + ch - '0', ch = getchar();
        return x * f;
    }
    template<typename T>
    inline void write(T x) {
        if (x < 0) x = -x, putchar('-');
        if (x > 9) write(x / 10);
        putchar(x % 10 + '0');
    }
    template<typename T>
    inline void writes(T x) { write(x), putchar(' '); }
    template<typename T>
    inline void writeln(T x) { write(x), putchar('\n'); }
} io;
```

## __int128

```cpp
ostream& operator<<(ostream& os, __int128 x) {
    if (x < 0) x = -x, os.put('-');
    if (x > 9) os << (x / 10);
    os.put(x % 10 + '0');
    return os;
}

istream& operator>>(istream& is, __int128& x) {
    x = 0;
    signed f = 1, ch = is.get();
    while (!isdigit(ch)) f = ch == '-' ? -1 : 1, ch = is.get();
    while (isdigit(ch)) x = x * 10 + ch - '0', ch = is.get();
    x *= f;
    return is;
}
```
