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

inline void remove(int x) {
    int u = (x % N + N) % N;
    if (h[u] == -1) return;
    if (e[h[u]] == x) h[u] = nex[h[u]];
    else
        for (int i = h[u];~i && ~nex[i]; i = nex[i])
            if (e[nex[i]] == x) nex[i] = nex[nex[i]];
}
```

## 开放寻址法
```cpp
int h[N], null = 0x3f3f3f3f;

inline void init() {
    memset(h, 0x3f, sizeof(h));
}

inline int get_index(int x){
    int u = (x % N + N) % N;
    while (h[u] != null && h[u] != x){
        u++;
        if (u == N) u = 0;
    }
    return u;
}

inline void insert(int x) {
    int u = get_index(x);
    h[u] = x;
}

inline bool find(int x) {
    int u = get_index(x);
    return h[u] == x;
}
```
