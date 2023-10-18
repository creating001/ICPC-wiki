# 字符串哈希

## 字符串匹配

> 板子题网址: https://www.luogu.com.cn/problem/P3370

```cpp
const int init = [](){
    p[0] = 1;
    for (int i = 1; str[i]; i++) {
        p[i] = p[i - 1] * P;
        h[i] = h[i - 1] * P + str[i];
    }
    return 0;
}();

inline ULL get(int l, int r) {
    return h[r] - h[l - 1] * p[r - l + 1];
}
```

## 允许 k 次失配的字符串匹配

> 板子题网址: https://www.luogu.com.cn/problem/P3805

```cpp

```

## 最长公共子字符串

> 板子题网址: https://www.luogu.com.cn/problem/SP1811
> 板子题网址: https://www.luogu.com.cn/problem/SP1812

```cpp

```

## 不同子字符串的数量

> 板子题网址: https://www.luogu.com.cn/problem/P2408

```cpp

```
