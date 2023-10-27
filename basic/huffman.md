# Huffman树

## 2 进制 Huffman 树

> 板子题网址: https://www.luogu.com.cn/problem/P1090
> 板子题网址: https://www.luogu.com.cn/problem/P6033

```cpp
inline LL solve(){
    for (int i = 0; i < N; i++)
        for (int j = 0; j < cnts[i]; j++)
            q1.emplace(i);
    LL ans = 0;
    auto get = [&]() -> LL {
        LL tmp;
        if (q2.empty() || (!q1.empty() && !q2.empty() && q1.front() < q2.front())) {
            tmp = q1.front();
            q1.pop();
        } else {
            tmp = q2.front();
            q2.pop();
        }
        return tmp;
    };
    while (q1.size() + q2.size() > 1) {
        LL a = get();
        LL b = get();
        ans += a + b;
        q2.emplace(a + b);
    }
    return ans;
}
```

## K 进制 Huffman 树

> 板子题网址: https://www.luogu.com.cn/problem/P2168

```cpp
inline PII solve() {
    LL ans = 0;
    while ((q.size() - 1) % (k - 1) != 0) q.emplace(0, 0);
    while (q.size() >= k) {
        LL h = -1, sum = 0;
        for (int i = 0; i < k; i++) {
            sum += q.top().F, h = max(h, q.top().S), q.pop();
        }
        ans += sum;
        q.emplace(sum, h + 1);
    }
    return {ans, q.top().S};
}
```
