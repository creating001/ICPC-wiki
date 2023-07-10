# 欧拉函数

欧拉函数 $\varphi(n)$ 是小于等于 $n$ 的正整数中与 $n$ 互质的数的个数。

1. $\varphi(1) = 1$
2. $\varphi(p) = p - 1$，其中 $p$ 为质数
3. $\varphi(n) = n \times \prod_{p | n} (1 - \frac{1}{p})$，其中 $p$ 为 $n$ 的质因子

