# 素数筛法

> 板子题网址: https://www.luogu.com.cn/problem/P3383

## 普通判素

时间复杂度: $O(N*\sqrt{N})$

```cpp
inline bool is_prime(int n) {
    if (n != 2 && n % 2 == 0) return false;
    for (int i = 3; i * i <= n; i += 2) {
        if (n % i == 0) return false;
    }
    return true;
}
```

## 埃氏筛

时间复杂度: $O(N*loglogN)$

```cpp
inline void get_prime(int n) {
    memset(is_prime, true, sizeof is_prime);
    for (int i = 2; i * i < n; i++) {
        if (!is_prime[i]) continue;
        for (int j = i * i; j < n; j += i)
            is_prime[j] = false;
    }
    for (int i = 2; i < n; i++)
        if (is_prime[i]) primes[prime_cnt++] = i;
}
```

## 欧拉筛

时间复杂度: $O(N)$

```cpp

```
