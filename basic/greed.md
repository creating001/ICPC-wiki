# 贪心

## 普通贪心

> 板子题网址: https://www.luogu.com.cn/problem/P4447

```cpp
for (int i = 1; i <= n; i++) cin >> a[i];
sort(a + 1, a + 1 + n);
multiset<PII> q;
for (int i = 1; i <= n; i++) {
    auto p = q.lower_bound({a[i] - 1, 0});
    if (p == q.end() || p->F != a[i] - 1) {
        q.insert({a[i], 1});
    } else {
        q.insert({a[i], p->S + 1});
        q.erase(p);
    }
}
int ans = INT_MAX;
for (auto& p : q)ans = min(ans, p.S);
```

## Huffman树

> 板子题网址: https://www.luogu.com.cn/problem/P6033
> 板子题网址: https://www.luogu.com.cn/problem/P2168

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
