# 欧拉函数

欧拉函数 $\varphi(n)$ 是小于等于 $n$ 的正整数中与 $n$ 互质的数的个数。

1. $\varphi(1) = 1$
2. $\varphi(p) = p - 1$，其中 $p$ 为质数
3. $\varphi(n) = n \times \prod_{p | n} (1 - \frac{1}{p})$，其中 $p$ 为 $n$ 的质因子

## 定义法求欧拉函数

> 板子题网址：https://www.acwing.com/problem/content/875

```cpp

时间复杂度: $O(\sqrt{N})$

```cpp
inline int get_phi(int x) {
    int ans = x;
    for (int i = 2; i <= x / i; i++)
        if (x % i == 0) {
            ans = ans / i * (i - 1);
            while (x % i == 0) x /= i;
        }
    if (x > 1) ans = ans / x * (x - 1);
    return ans;
}
```

## 线性筛法求欧拉函数

> 板子题网址：https://www.acwing.com/problem/content/876

时间复杂度：$O(N)$

```cpp
inline void get_primes(int n) {
    phi[1] = 1;
    memset(is_prime, 1, sizeof(is_prime));
    for (int i = 2; i <= n; i++) {
        if (is_prime[i]) primes[cnts++] = i, phi[i] = i - 1;
        for (int j = 0; primes[j] <= n / i; j++) {
            is_prime[primes[j] * i] = false;
            if (i % primes[j] == 0) {
                phi[primes[j] * i] = phi[i] * primes[j];
                break;
            }
            phi[primes[j] * i] = phi[i] * (primes[j] - 1);
        }
    }
}
```

## 欧拉定理

定理: 若 $a$ 和 $n$ 互质，则 $a^{\varphi(n)} \equiv 1 \pmod{n}$。

推论: 若 $a$ 和 $n$ 互质，则 $a^x \equiv a^{x \bmod \varphi(n)} \pmod{n}$。

证明过程:
1. 假设 $[1, n]$ 中与 $n$ 互质的数为 $p_1, p_2, \cdots, p_{\varphi(n)}$
2. 因为 $a$ 与 $n$ 互质, 所以 $a \times p_1, a \times p_2, \cdots, a \times p_{\varphi(n)}$ 与 $p_1, p_2, \cdots, p_{\varphi(n)}$ 在模 $n$ 意义下相等
3. 因为 $p_1, p_2, \cdots, p_{\varphi(n)}$ 两两互质，所以 $a^{\varphi(n)} \times p_1 \times p_2 \times \cdots \times p_{\varphi(n)} \equiv p_1 \times p_2 \times \cdots \times p_{\varphi(n)} \pmod{n}$
4. 所以 $a^{\varphi(n)} \equiv 1 \pmod{n}$

推论证明:
1. 易知 $a^x$ 可以改写为 $a^{ k \times \varphi(n) + x \bmod \varphi(n)}$
2. 因为 $a^{\varphi(n)} \equiv 1 \pmod{n}$
3. 所以 $a^{ k \times \varphi(n) + x \bmod \varphi(n)} \equiv a^{x \bmod \varphi(n)} \pmod{n}$
4. 所以 $a^x \equiv a^{x \bmod \varphi(n)} \pmod{n}$

## 欧拉降幂(扩展欧拉定理)

> 板子题网址: https://www.luogu.com.cn/problem/P5091

欧拉降幂定理: 若 $a$ 和 $n$ 互质，则 $a^x \equiv a^{x \bmod \varphi(n) + \varphi(n)} \pmod{n}$。

```cpp
inline int quick_pow(LL a, int b, int p) {
    if (b > 2 * phi[p]) b = b % phi[p] + phi[p];
    int ans = 1;
    while (b) {
        if (b & 1) ans = ans * a % p;
        a = a * a % p;
        b >>= 1;
    }
    return ans;
}
```

## 欧拉几何公式

公式: V-E+F = 2

板子题网址: https://www.luogu.com.cn/problem/U323480

```cpp

```
