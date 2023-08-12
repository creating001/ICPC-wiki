# 快读快写

> 板子题网址: https://www.luogu.com.cn/problem/U183572

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
