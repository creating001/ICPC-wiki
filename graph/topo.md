# 拓扑排序

拓扑排序要解决的问题是给一个有向无环图的所有节点排序。

> 板子题网址: https://www.luogu.com.cn/problem/B3644

```cpp
inline bool topo_sort() {
    queue<int> q;
    for (int i = 1; i <= n; i++)
        if (!d[i]) q.emplace(i), vis[i] = 1;
    while (!q.empty()) {
        int cur = q.front();
        q.pop();
        a[cnts++] = cur;
        for (int i = h[cur]; i; i = nex[i]) {
            int to = e[i];
            if (vis[to]) continue;
            d[to]--;
            if (!d[to]) q.emplace(to), vis[to] = 1;
        }
    }
    return cnts == n;
}
```
