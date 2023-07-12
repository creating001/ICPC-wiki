# DFS

> 板子题网址: https://www.luogu.com.cn/problem/P1706

在 搜索算法 中，该词常常指利用递归函数方便地实现暴力枚举的算法，与图论中的 DFS 算法有一定相似之处，但并不完全相同。

## 代码模板

```cpp
inline void dfs(int index) {
    if (index == n) {
        for (int i = 0; i < n; i++) {
            cout << arr[i] << " ";
        }
    }
    for (int i = 1; i <= n; i++) {
        if (!vis[i]) {
            vis[i] = 1;
            arr.push_back(i);
            dfs(index + 1);
            arr.pop_back();
            vis[i] = 0;
        }
    }
}
```
