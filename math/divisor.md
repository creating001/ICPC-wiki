# 约数

## 试除法求约数

时间复杂度: $O(\sqrt{N})$

```cpp
inline vector<int> get_divisors(int x) {
    vector<int> res;
    for (int i = 1; i <= x / i; i++)
        if (x % i == 0) {
            res.emplace_back(i);
            if (i != x / i) res.emplace_back(x / i);
        }
    sort(res.begin(), res.end());
    return res;
}
```

## 约数个数

> 板子题网址: https://www.acwing.com/problem/content/872

原理：
$N = p_1^{a_1} \times p_2^{a_2} \times \cdots \times p_k^{a_k}$，则约数个数为 $(a_1 + 1) \times (a_2 + 1) \times \cdots \times (a_k + 1)$

时间复杂度: $O(\sqrt{N})$

```cpp
inline int get_divisor_count(int x) {
    int res = 1;
    for (int i = 2; i <= x / i; i++) {
        int s = 0;
        while (x % i == 0) x /= i, s++;
        res *= s + 1;
    }
    if (x > 1) res *= 2;
    return res;
}
```

## 约数之和

> 板子题网址: https://www.acwing.com/problem/content/873

原理:
$N = p_1^{a_1} \times p_2^{a_2} \times \cdots \times p_k^{a_k}$，则约数之和为 $\prod_{i = 1}^k \sum_{j = 0}^{a_i} p_i^j$, 利用等比数列求和公式可得 $\prod_{i = 1}^k \frac{p_i^{a_i + 1} - 1}{p_i - 1}$

时间复杂度: $O(\sqrt{N})$

```cpp
inline int get_divisor_sum(int x) {
    int res = 1;
    for (int i = 2; i <= x / i; i++) {
        int t = 1;
        while (x % i == 0) x /= i, t = t * i + 1;
        res *= t;
    }
    if (x > 1) res *= x + 1;
    return res;
}
```
