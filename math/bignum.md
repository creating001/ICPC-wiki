# 高精度算法

> 板子题网址: https://www.luogu.com.cn/problem/P1932

## 加法

> 板子题网址: https://www.luogu.com.cn/problem/P1601

```cpp
inline vector<int> add(vector<int>& A, vector<int> B) {
    if (A.size() < B.size()) return add(B, A);
    vector<int> ans(A.size() + 1);
    for (int i = 0, t = 0; i < A.size(); i++) {
        if (i < A.size()) t += A[i];
        if (i < B.size()) t += B[i];
        ans[i] = t % 10, t /= 10;
    }
    while (ans.back() == 0 && ans.size() > 1) ans.pop_back();
    return ans;
}
```

## 减法

> 板子题网址: https://www.luogu.com.cn/problem/P2142

```cpp
inline bool greater_eq(vector<int>& A, vector<int>& B) {
    if (A.size() != B.size()) return A.size() > B.size();
    for (int i = A.size() - 1; i >= 0; i--)
        if (A[i] != B[i]) return A[i] > B[i];
    return true;
}

inline vector<int> sub(vector<int>& A, vector<int>& B) {
    vector<int> ans;
    for (int i = 0, t = 0; i < A.size(); i++) {
        t = A[i] - t;
        if (i < B.size()) t -= B[i];
        ans.emplace_back((t + 10) % 10);
        t = t < 0 ? 1 : 0;
    }
    while (ans.back() == 0 && ans.size() > 1) ans.pop_back();
    return ans;
}
```

## 乘法

### 高精 * 低精

> 板子题网址: https://www.acwing.com/problem/content/795/

```cpp
inline vector<int> mul(vector<int>& A, int b) {
    vector<int> ans;
    for (int i = 0, t = 0; i < A.size() || t; i++) {
        if (i < A.size()) t += A[i] * b;
        ans.emplace_back(t % 10);
        t /= 10;
    }
    while (ans.back() == 0 && ans.size() > 1) ans.pop_back();
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
    for (int i = 0, t = 0; i < ans.size(); i++)
        ans[i] += t, t = ans[i] / 10, ans[i] %= 10;
    while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
    return ans;
}
```

## 除法

## 高精 / 低精

> 板子题网址: https://www.luogu.com.cn/problem/U289291

```cpp
inline vector<int> div(vector<int>& A, int b, int& r) {
    r = 0;
    vector<int> ans;
    for (int i = A.size() - 1; i >= 0; i--)
        r = r * 10 + A[i], ans.emplace_back(r / b), r %= b;
    reverse(ans.begin(), ans.end());
    while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
    return ans;
}
```

## 高精 / 高精

> 板子题网址: https://www.luogu.com.cn/problem/P2005

```cpp
inline void clear_end(vector<int>& arr) {
    while (arr.back() == 0 && arr.size() > 1) arr.pop_back();
}

inline bool greater_eq(vector<int>& A, vector<int>& B, int last_dg) {
    if (B.size() + last_dg < A.size() && A[B.size() + last_dg])
        return true;
    for (int i = B.size() - 1; i >= 0; i--)
        if (A[i + last_dg] != B[i]) return A[i + last_dg] > B[i];
    return true;
}

inline vector<int> div(vector<int>& A, vector<int>& B, vector<int>& R) {
    vector<int> ans(A.size() - B.size() + 1);
    R = A;
    for (int i = A.size() - B.size(); i >= 0; i--)
        while (greater_eq(R, B, i)) {
            for (int j = 0; j < B.size(); j++)
                if ((R[i + j] -= B[j]) < 0) R[i + j + 1] -= 1, R[i + j] += 10;
            ans[i]++;
        }
    clear_end(ans), clear_end(R);
    return ans;
}
```
