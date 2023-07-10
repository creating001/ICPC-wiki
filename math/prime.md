# 素数

## 试除法判素

时间复杂度: $O(\sqrt{N})$

```cpp
inline bool is_prime(int x) {
    if (x < 2 || (x > 2 && x % 2 == 0)) return false;
    for (int i = 3; i <= x / i; i += 2)
        if (x % i == 0) return false;
    return true;
}
```

## 质因数分解

时间复杂度: $O(\sqrt{N})$

```cpp
inline void divide(int x) {
    for (int i = 2; i <= x / i; i++) {
        int s = 0;
        while (x % i == 0) x /= i, s++;
    }
    if (x > 1){/* x 是质数 */}
}
```

## 埃氏筛

> 板子题网址: https://www.luogu.com.cn/problem/P3383

时间复杂度: $O(N \times \log \log N)$

```cpp
inline void get_primes(int n) {
    memset(is_prime, 1, sizeof(is_prime));
    for (int i = 2; i <= n; i++) {
        if (!is_prime[i]) continue;
        primes[cnt++] = i;
        for (int j = i + i; j <= n; j += i)
            is_prime[j] = false;
    }
}
```

## 欧拉筛

> 板子题网址: https://www.luogu.com.cn/problem/P3912

时间复杂度: $O(N)$

```cpp
inline void get_prime(int n) {
    memset(is_prime, 1, sizeof(is_prime));
    for (int i = 2; i <= n; i++) {
        if (is_prime[i]) primes[cnts++] = i;
        for (int j = 0; primes[j] <= n / i; j++) {
            is_prime[primes[j] * i] = false;
            if (i % primes[j] == 0) break;
        }
    }
}
```
