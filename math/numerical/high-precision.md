# 高精度算法

> 板子题网址: https://www.luogu.com.cn/problem/P1932

## 加法

> 板子题网址: https://www.luogu.com.cn/problem/P1601

```cpp
inline vector<int> add(vector<int>& A, vector<int>& B) {
    int len = max(A.size(), B.size());
    vector<int> ans(len + 1);
    for (int i = 0; i < len; i++) {
        if (i < A.size()) ans[i] += A[i];
        if (i < B.size()) ans[i] += B[i];
        ans[i + 1] += ans[i] / 10;
        ans[i] %= 10;
    }
    while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
    return ans;
}
```

## 减法

> 板子题网址: https://www.luogu.com.cn/problem/P2142

```cpp
inline vector<int> sub(vector<int>& A, vector<int>& B) {
    vector<int> ans(A.size());
    for (int i = 0; i < A.size(); i++) {
        ans[i] += A[i];
        if (i < B.size()) ans[i] -= B[i];
        if (ans[i] < 0) ans[i] += 10, ans[i + 1] -= 1;
    }
    while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
    return ans;
}
```

## 乘法

### 高精 * 低精

> 板子题网址: https://www.acwing.com/problem/content/795

```cpp
inline vector<int> mul(vector<int>& A, int b) {
    if (b == 0) return {0};
    vector<int> ans(A.size() + log2(b) + 1);
    for (int i = 0; i < ans.size() - 1; i++) {
        if (i < A.size()) ans[i] += A[i] * b;
        ans[i + 1] += ans[i] / 10;
        ans[i] %= 10;
    }
    while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
    return ans;
}
```

### 高精 * 高精

> 板子题网址: https://www.luogu.com.cn/problem/P1303

```cpp
inline vector<int> mul(vector<int>& A, vector<int>& B) {
    vector<int> ans(A.size() + B.size());
    for (int i = 0; i < A.size(); i++)
        for (int j = 0; j < B.size(); j++)
            ans[i + j] += A[i] * B[j];
    for (int i = 0; i < ans.size() - 1; i++) {
        ans[i + 1] += ans[i] / 10;
        ans[i] %= 10;
    }
    while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
    return ans;
}
```

## 除法

### 高精 / 低精

> 板子题网址: https://www.luogu.com.cn/problem/U289291

```cpp
inline vector<int> div(vector<int>& A, int b, int& r) {
    r = 0;
    vector<int> ans(A.size());
    for (int i = A.size() - 1; i >= 0; i--)
        r = r * 10 + A[i], ans[i] = r / b, r %= b;
    while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
    return ans;
}
```

### 高精 / 高精

> 板子题网址: https://www.luogu.com.cn/problem/P2005

```cpp
inline bool div_greater(vector<int>& A, vector<int>& B, int last_dg) {
    if (B.size() + last_dg < A.size() && A[B.size() + last_dg])
        return true;
    for (int i = B.size() - 1; i >= 0; i--)
        if (A[i + last_dg] != B[i]) return A[i + last_dg] > B[i];
    return true;
}

inline vector<int> div(vector<int>& A, vector<int>& B, vector<int>& R) {
    R = A;
    vector<int> ans(max((int) (A.size() - B.size() + 1), 1));
    for (int i = ans.size() - 1; i >= 0; i--)
        while (div_greater(R, B, i)) {
            for (int j = 0; j < B.size(); j++) {
                R[i + j] -= B[j];
                if (R[i + j] < 0) R[i + j] += 10, R[i + j + 1] -= 1;
            }
            ans[i]++;
        }
    while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
    while (R.size() > 1 && R.back() == 0) R.pop_back();
    return ans;
}
```
