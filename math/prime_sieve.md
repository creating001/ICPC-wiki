# 素数筛法

> 板子题网址: https://www.luogu.com.cn/problem/P3383
>
> 板子题网址: https://www.luogu.com.cn/problem/P3912

## 试除法

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

时间复杂度: $O(N \times \log \log N)$

```cpp
inline void get_prime(int n) {
    memset(is_prime, 1, sizeof(is_prime));
    for (int i = 2; i <= n; i++) {
        if (!is_prime[i]) continue;
        for (int j = i + i; j <= n; j += i)
            is_prime[j] = false;
    }
}
```

## 欧拉筛

时间复杂度: $O(N)$

```cpp

```

## 速度测试

> 以下代码均使用 g++-4.8 code.cpp -O2 命令行编译，CPU 使用 Intel i5-8259U 进行测试。测试结果取十次平均值。

| 算法              | $5 \times 10^7$ | $10^8$ | $5 \times 10^8$ |
| ----------------- | --------------- | ------ | --------------- |
| 埃氏筛 + 布尔数组 | 386ms           | 773ms  | 4.41s           |
| 欧拉筛 + 布尔数组 | 257ms           | 521ms  | 2.70s           |
| 埃氏筛 + bitset   | 219ms           | 492ms  | 2.66s           |
| 欧拉筛 + bitset   | 332ms           | 661ms  | 3.21s           |

从测试结果中可知，时间复杂度 $O(n \log \log n)$ 的埃氏筛在使用 bitset 优化后速度甚至超过时间复杂度 $O(n)$ 的欧拉筛，而欧拉筛在使用 bitset 后会出现「负优化」的情况。
