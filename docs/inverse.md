### 线性求逆元

$$
i^{-1} \equiv \begin{cases}
    1,                                           & \text{if } i = 1, \\
    -\lfloor\frac{p}{i}\rfloor (p \bmod i)^{-1}, & \text{otherwise}.
\end{cases} \pmod p
$$
