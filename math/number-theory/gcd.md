# 欧几里得

时间复杂度: $O(\log{N})$

## 辗转相除法

```cpp
inline int gcd(int a, int b) {
    return b ? gcd(b, a % b) : a;
}
```

## 裴蜀定理

> 板子题网址: https://www.luogu.com.cn/problem/P4549

定理: 对于任意的整数 $a, b, c$，方程 $ax + by = c$ 有整数解当且仅当 $gcd(a, b) | c$ 成立。

证明:
1. 充分性证明: 扩展欧几里得算法构造。
2. 必要性证明: $gcd(a, b) | a$ 且 $gcd(a, b) | b$ 成立，所以 $gcd(a, b) | ax + by$ 成立。

全部解:
$x = x_0 + \frac{b}{gcd(a, b)}t, y = y_0 - \frac{a}{gcd(a, b)}t$，其中 $x_0, y_0$ 为一组特解。

## 扩展欧几里得算法

> 板子题网址: https://www.luogu.com.cn/problem/P2613
>
> 板子题网址: https://www.luogu.com.cn/problem/P5656

```cpp
inline int exgcd(int a, int b, int &x, int &y) {
    if (!b) {
        x = 1, y = 0;
        return a;
    }
    int d = exgcd(b, a % b, y, x);
    y = y - a / b * x;
    return d;
}
```

推导过程: $ax + by = d$
1. $b = 0$ 时 $ax + by = d$ , 则 $x = 1, y = 0$
2. - $b \neq 0$ 时，我们已经已知 $by + (a \bmod b)x = d$ 成立, 我们需要构造 $ax' + by' = d$ 成立
   - 所以可得 $x' = x$, $y' = y - a / b * x$

## [类欧几里得算法](https://oi-wiki.org/math/number-theory/euclidean)

> 板子题网址: https://www.luogu.com.cn/problem/P5170

```cpp

```
