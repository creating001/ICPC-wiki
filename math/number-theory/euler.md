# 欧拉

## 欧拉定理

若 $\gcd(a, m) = 1$，则 $a^{\varphi(m)} \equiv 1 \pmod{m}$。

## 扩展欧拉定理

$$
a^b \equiv \begin{cases}
  a^{b \bmod \varphi(m)},                &\gcd(a,m) =  1,                   \\
  a^b,                                   &\gcd(a,m)\ne 1, b <   \varphi(m), \\
  a^{(b \bmod \varphi(m)) + \varphi(m)}, &\gcd(a,m)\ne 1, b \ge \varphi(m).
\end{cases} \pmod m
$$
