# 快速幂

公式原理: $a^k = a^{2^{k_1}} \times a^{2^{k_2}} \times \cdots \times a^{2^{k_n}}$

时间复杂度: $O(\log k)$

## 代码实现
```cpp
inline LL quick_pow(LL a, int k, int p) {
    LL ans = 1;
    while (k) {
        if (k & 1) ans = ans * a % p;
        a = a * a % p;
        k >>= 1;
    }
    return ans;
}
```

## 快速幂求逆元

> 逆元: $a \times a^{-1} \equiv 1 \pmod p$

```cpp
inline LL quick_pow(LL a, int k, int p) {
    LL ans = 1;
    while (k) {
        if (k & 1) ans = ans * a % p;
        a = a * a % p;
        k >>= 1;
    }
    return ans;
}
// a 和 p 互质 且 p 为质数
inline LL inv(int a, int p) {
    return quick_pow(a, p - 2, p);
}
```
