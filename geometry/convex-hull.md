# 凸包

## 二维凸包 Andrew 算法

> 板子题网址: https://www.luogu.com.cn/problem/P2742

```cpp
inline double andrew() {
    sort(p, p + idx);
    for (int i = 0; i < idx; i++) {
        while (top >= 2 && area(stk[top - 1], stk[top], p[i]) <= 0)
            top--;
        stk[++top] = p[i];
    }
    int m = top;
    for (int i = idx - 2; i >= 0; i--) {
        while (top >= m + 1 && area(stk[top - 1], stk[top], p[i]) <= 0)
            top--;
        stk[++top] = p[i];
    }
    double ans = 0;
    for (int i = 1; i < top; i++) {
        ans += dis(stk[i], stk[i + 1]);
    }
    return ans;
}
```

