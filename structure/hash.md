# 哈希表

## 链表法
```cpp
int h[N], e[N], nex[N], idx;

inline void init() {
    memset(h, -1, sizeof(h));
}

inline void insert(int x) {
    int u = (x % N + N) % N;
    for (int i = h[u]; ~i; i = nex[i])
        if (e[i] == x) return;
    e[idx] = x, nex[idx] = h[u], h[u] = idx;
    idx++;
}

inline bool find(int x) {
    int u = (x % N + N) % N;
    for (int i = h[u]; ~i; i = nex[i])
        if (e[i] == x) return true;
    return false;
}
```

## 开放寻址法
```cpp
int h[N], null = 0x3f3f3f3f;

inline void init() {
    memset(h, 0x3f, sizeof(h));
}

inline void insert(int x) {
    int u = (x % N + N) % N;
    while (h[u] != null && h[u] != x)
        u++;
    h[u] = x;
}

inline bool find(int x) {
    int u = (x % N + N) % N;
    while (h[u] != null && h[u] != x)
        u++;
    return h[u] == x;
}
```
