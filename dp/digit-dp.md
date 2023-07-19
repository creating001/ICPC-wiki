# 数位DP

数位是指把一个数字按照个、十、百、千等等一位一位地拆开，关注它每一位上的数字。如果拆的是十进制数，那么每一位数字都是 $[0, 9]$ ，其他进制可类比十进制。

数位 DP 的基本原理:

考虑人类计数的方式，最朴素的计数就是从小到大开始依次加一。但我们发现对于位数比较多的数，这样的过程中有许多重复的部分。例如，从 7000 数到 7999 和从 9000 数到 9999 的过程非常相似，它们都是后三位从 000 变到 999，不一样的地方只有千位这一位，所以我们可以把这些过程归并起来，将这些过程中产生的计数答案也都存在一个通用的数组里。此数组根据题目具体要求设置状态，用递推或 DP 的方式进行状态转移。

数位 DP 中通常会利用常规计数问题技巧，比如把一个区间内的答案拆成两部分相减 ( 即 $\mathit{ans}_{[l, r]} = \mathit{ans}_{[0, r]}-\mathit{ans}_{[0, l - 1]}$ )

## 统计数位

> 板子题网址: https://www.luogu.com.cn/problem/P2602

```cpp
inline int get(vector<int>& a, int r, int l) {
    int ans = 0;
    for (int i = r; i >= l; i--)
        ans = ans * 10 + a[i];
    return ans;
}

inline int pow10(int k) {
    int ans = 1;
    while (k--) ans *= 10;
    return ans;
}

inline int count(int n, int x) {
    vector<int> a;
    while (n) a.emplace_back(n % 10), n /= 10;
    n = (int) a.size();
    int ans = 0;
    for (int i = n - 1 - !x; i >= 0; i--) {
        if (i < n - 1)
            ans += (get(a, n - 1, i + 1) - !x) * pow10(i);
        if (a[i] == x)
            ans += get(a, i - 1, 0) + 1;
        else if (a[i] > x)
            ans += pow10(i);
    }
    return ans;
}
```
