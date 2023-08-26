# 自动机

## AC自动机

> 板子题网址: https://www.luogu.com.cn/problem/P3796

```cpp
inline void insert(int x) {
    int p = 0;
    for (int i = 0; s[i]; i++) {
        int u = s[i] - 'a';
        if (!tr[p][u]) tr[p][u] = ++idx;
        p = tr[p][u];
    }
    id[p] = x;
}

inline void build() {
    queue<int> q;
    for (int i = 0; i < 26; i++)
        if (tr[0][i]) q.emplace(tr[0][i]);
    while (!q.empty()) {
        int t = q.front();
        q.pop();
        for (int i = 0; i < 26; i++) {
            int p = tr[t][i];
            if (!p) {
                tr[t][i] = tr[nex[t]][i];
                continue;
            }
            nex[p] = tr[nex[t]][i];
            q.emplace(p);
        }
    }
}
```
