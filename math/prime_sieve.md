# 素数筛法

> 板子题网址1: https://www.luogu.com.cn/problem/P3383
>
> 板子题网址2: https://leetcode.cn/problems/count-primes

## 普通判素

时间复杂度: $O(N \times \sqrt{N})$

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

时间复杂度: $O(N \times \log \log N)$

```cpp
const int N = 1e8;
int primes[N];
int prime_cnt = 0;
bitset<N> is_prime;
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

速度测试
以下代码均使用 g++-4.8 code.cpp -O2 命令行编译，CPU 使用 Intel i5-8259U 进行测试。测试结果取十次平均值。

| 算法              | `5e7` | `1e8` | `5e8` |
| ----------------- | ----- | ----- | ----- |
| 埃氏筛 + 布尔数组 | 386ms | 773ms | 4.41s |
| 欧拉筛 + 布尔数组 | 257ms | 521ms | 2.70s |
| 埃氏筛 +bitset    | 219ms | 492ms | 2.66s |
| 欧拉筛 +bitset    | 332ms | 661ms | 3.21s |

从测试结果中可知，时间复杂度 $O(n \log \log n)$ 的埃氏筛在使用 bitset 优化后速度甚至超过时间复杂度 O(n) 的欧拉筛，而欧拉筛在使用 bitset 后会出现「负优化」的情况。
