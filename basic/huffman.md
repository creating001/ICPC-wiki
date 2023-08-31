# Huffman树

## 果子合并 一般版

> 板子题网址: https://www.luogu.com.cn/problem/P1090

```cpp
inline int solve(){
    for (int i = 0; i < n; i++)
        q.emplace(a[i]);
    int ans = 0;
    while (q.size() > 1) {
        int a = q.top();
        q.pop();
        int b = q.top();
        q.pop();
        ans += a + b;
        q.emplace(a + b);
    }
    return ans;
}
```

## 果子合并 强化版

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
