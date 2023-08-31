# 素数

## 质因数分解

> 板子题网址: https://www.acwing.com/problem/content/869

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

```cpp
const int init = [](int n) {
    memset(is_prime, 1, sizeof(is_prime));
    for (int i = 2; i <= n; i++) {
        if (!is_prime[i]) continue;
        prime[cnts++] = i;
        for (int j = i + i; j <= n; j += i) is_prime[j] = false;
    }
    return 0;
}(N - 1);
```

## 欧拉筛

> 板子题网址: https://www.luogu.com.cn/problem/P3912

```cpp
const int init = [](int n) {
    memset(is_prime, 1, sizeof(is_prime));
    for (int i = 2; i <= n; i++) {
        if (is_prime[i]) prime[cnts++] = i;
        for (int j = 0; prime[j] <= n / i; j++) {
            is_prime[prime[j] * i] = false;
            if (i % prime[j] == 0) break;
        }
    }
    return 0;
}(N - 1);
```

## 哥德巴赫猜想

任一大于2的偶数都可写成两个质数之和。

> 板子题网址: https://www.luogu.com.cn/problem/P1579

```cpp

```