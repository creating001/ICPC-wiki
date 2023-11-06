# 素数

## 质因数分解

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
    for (int i = 2; i <= n; i++) {
        if (vis[i]) continue;
        prime[cnts++] = i;
        for (int j = i + i; j <= n; j += i) vis[j] = true;
    }
    return 0;
}(N - 1);
```

## 欧拉筛

> 板子题网址: https://www.luogu.com.cn/problem/P3912

```cpp
const int init = [](int n) {
    for (int i = 2; i <= n; i++) {
        if (!vis[i]) prime[cnts++] = i;
        for (int j = 0; prime[j] <= n / i; j++) {
            vis[prime[j] * i] = true;
            if (i % prime[j] == 0) break;
        }
    }
    return 0;
}(N - 1);
```

## 哥德巴赫猜想

> 板子题网址: https://www.luogu.com.cn/problem/P1579

```cpp

```

## 威尔逊定理

$(p-1)!\equiv -1\pmod p$ 当且仅当 $p$ 是质数
