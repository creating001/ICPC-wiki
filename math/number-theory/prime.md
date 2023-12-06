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

## 哥德巴赫猜想

> 板子题网址: https://www.luogu.com.cn/problem/P1579

```cpp

```

## 威尔逊定理

$(p-1)!\equiv -1\pmod p$ 当且仅当 $p$ 是质数
