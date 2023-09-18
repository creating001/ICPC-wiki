# 栈和队列

## 单调栈

> 板子题网址: https://www.luogu.com.cn/problem/P5788

```cpp

```

## 单调队列

> 板子题网址: https://www.luogu.com.cn/problem/P1886

```cpp

```

## 二维单调队列

> 板子题网址: https://www.luogu.com.cn/problem/P2216

```cpp
inline void solve() {
    for (int i = 1; i <= a; i++) {
        int hh = 0, tt = -1;
        for (int j = 1; j <= b; j++) {
            while (hh <= tt && q[hh] < j - n + 1) hh++;
            while (hh <= tt && g[i][q[tt]] <= g[i][j]) tt--;
            q[++tt] = j;
            mx[i][j] = g[i][q[hh]];
        }
    }

    for (int i = 1; i <= a; i++) {
        int hh = 0, tt = -1;
        for (int j = 1; j <= b; j++) {
            while (hh <= tt && q[hh] < j - n + 1) hh++;
            while (hh <= tt && g[i][q[tt]] >= g[i][j]) tt--;
            q[++tt] = j;
            mn[i][j] = g[i][q[hh]];
        }
    }


    memcpy(g, mx, sizeof(mx));
    for (int j = 1; j <= b; j++) {
        int hh = 0, tt = -1;
        for (int i = 1; i <= a; i++) {
            while (hh <= tt && q[hh] < i - n + 1) hh++;
            while (hh <= tt && g[q[tt]][j] <= g[i][j]) tt--;
            q[++tt] = i;
            mx[i][j] = g[q[hh]][j];
        }
    }

    memcpy(g, mn, sizeof(mn));
    for (int j = 1; j <= b; j++) {
        int hh = 0, tt = -1;
        for (int i = 1; i <= a; i++) {
            while (hh <= tt && q[hh] < i - n + 1) hh++;
            while (hh <= tt && g[q[tt]][j] >= g[i][j]) tt--;
            q[++tt] = i;
            mn[i][j] = g[q[hh]][j];
        }
    }

    int ans = INF;
    for (int i = n; i <= a; i++) for (int j = n; j <= b; j++) ans = min(ans, mx[i][j] - mn[i][j]);
}
```
