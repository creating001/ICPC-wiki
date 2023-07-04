# 字典树

> 板子题网址: https://www.luogu.com.cn/problem/P8306

## 数据结构模拟
```cpp
// N为最大节点数, M为字符集大小, idx为当前节点数, cnts为当前节点的字符串数量
int son[N][M], idx, cnts[N];
```

## 插入和查询
```cpp
inline void insert(string& str) {
    int p = 0;
    for (char i : str) {
        int u = i - 'a';
        if (!son[p][u]) son[p][u] = idx++;
        p = son[p][u];
    }
    cnts[p]++;
}

inline int query(string& str) {
    int p = 0;
    for (char i : str) {
        int u = i - 'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnts[p];
}
```
