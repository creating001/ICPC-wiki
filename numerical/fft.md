# FFT

> 板子题网址: https://www.luogu.com.cn/problem/P3803

```cpp
inline void FFT(CD* f, int inv) {
    for (int i = 0; i < tot; i++)
        if (i < rev[i]) swap(f[i], f[rev[i]]);
    for (int mid = 1; mid < tot; mid <<= 1) {
        CD w = {cos(PI / mid), inv * sin(PI / mid)};
        for (int i = 0; i < tot; i += mid << 1) {
            CD wk = {1, 0};
            for (int j = 0; j < mid; j++, wk = wk * w) {
                CD x = f[i + j], y = wk * f[i + j + mid];
                f[i + j] = x + y, f[i + j + mid] = x - y;
            }
        }
    }
}
```
