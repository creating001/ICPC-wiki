# 字典树

> 板子题网址: https://www.luogu.com.cn/problem/P8306
>
> 板子题网址: https://www.luogu.com.cn/problem/P4551

## 数据存储
```cpp
int son[N][M], idx, cnts[N];
```

## 字典树操作
```cpp
inline int getIndex(char ch) {
    if (isdigit(ch)) return ch - '0';
    if (islower(ch)) return ch - 'a' + 10;
    return ch - 'A' + 36;
}

inline void insert(char* str) {
    for (int i = 0, p = 0; str[i]; i++) {
        int u = getIndex(str[i]);
        if (!son[p][u]) son[p][u] = ++idx;
        p = son[p][u];
        cnts[p]++;
    }
}

inline int query(char* str) {
    int p = 0;
    for (int i = 0; str[i]; i++) {
        int u = getIndex(str[i]);
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnts[p];
}
```
