# 素数筛法

## 埃氏筛

> 板子题网址: https://www.luogu.com.cn/problem/P3383

```cpp
const int init = [](int n) {
    for (int i = 2; i <= n; i++) {
        if (vis[i]) continue;
        prime[cnts++] = i;
        for (int j = i + i; j <= n; j += i) vis[j] = true;
    }
    return 0;
}(N - 1);
```

## 欧拉筛

> 板子题网址: https://www.luogu.com.cn/problem/P3912

```cpp
const int init = [](int n) {
    for (int i = 2; i <= n; i++) {
        if (!vis[i]) prime[cnts++] = i;
        for (int j = 0; prime[j] <= n / i; j++) {
            vis[prime[j] * i] = true;
            if (i % prime[j] == 0) break;
        }
    }
    return 0;
}(N - 1);
```
