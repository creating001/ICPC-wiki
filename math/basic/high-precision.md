# 高精度算法

> 板子题网址: https://www.luogu.com.cn/problem/P1932

## 处理
```cpp
inline void clear(vector<int>& arr) {
    while (arr.size() > 1 && arr.back() == 0) arr.pop_back();
}

inline vector<int> read() {
    static string str;
    cin >> str;
    vector<int> ans(str.size());
    for (int i = str.size() - 1; i >= 0; i--)
        ans[str.size() - i - 1] = str[i] - '0';
    return clear(ans), ans;
}

inline void write(vector<int>& arr) {
    for (int i = arr.size() - 1; i >= 0; i--) cout << arr[i];
    cout << endl;
}

inline bool greater_eq(vector<int>& A, vector<int>& B) {
    if (A.size() != B.size()) return A.size() > B.size();
    for (int i = A.size() - 1; i >= 0; i--)
        if (A[i] != B[i]) return A[i] > B[i];
    return true;
}

inline bool greater_eq(vector<int>& A, vector<int>& B, int last_dg) {
    if (B.size() + last_dg < A.size() && A[B.size() + last_dg])
        return true;
    for (int i = B.size() - 1; i >= 0; i--)
        if (A[i + last_dg] != B[i]) return A[i + last_dg] > B[i];
    return true;
}
```

## 加法

> 板子题网址: https://www.luogu.com.cn/problem/P1601

```cpp
inline vector<int> add(vector<int>& A, vector<int>& B) {
    if (A.size() < B.size()) return add(B, A);
    vector<int> ans(A.size() + 1);
    for (int i = 0, t = 0; i < A.size() || t; i++) {
        if (i < A.size()) t += A[i];
        if (i < B.size()) t += B[i];
        ans[i] = t % 10, t /= 10;
    }
    return clear(ans), ans;
}
```

## 减法

> 板子题网址: https://www.luogu.com.cn/problem/P2142

```cpp
inline vector<int> sub(vector<int>& A, vector<int>& B) {
    vector<int> ans(A.size());
    for (int i = 0; i < A.size(); i++) {
        ans[i] += A[i] - (i < B.size() ? B[i] : 0);
        if (ans[i] < 0) ans[i] += 10, ans[i + 1] -= 1;
    }
    return clear(ans), ans;
}
```

## 乘法

### 高精 * 低精

> 板子题网址: https://www.acwing.com/problem/content/795

```cpp
inline vector<int> mul(vector<int>& A, int b) {
    vector<int> ans(A.size() + log10(b + 1) + 1);
    for (int i = 0, t = 0; i < A.size() || t; i++) {
        if (i < A.size()) t += A[i] * b;
        ans[i] += t, t = ans[i] / 10, ans[i] %= 10;
    }
    return clear(ans), ans;
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
    return clear(ans), ans;
}
```

## 除法

### 高精 / 低精

> 板子题网址: https://www.luogu.com.cn/problem/U289291

```cpp
inline vector<int> div(vector<int>& A, int b, long long& r) {
    r = 0;
    vector<int> ans(A.size());
    for (int i = A.size() - 1; i >= 0; i--)
        r = r * 10 + A[i], ans[i] = r / b, r %= b;
    return clear(ans), ans;
}
```

### 高精 / 高精

> 板子题网址: https://www.luogu.com.cn/problem/P2005

```cpp
inline vector<int> div(vector<int>& A, vector<int>& B, vector<int>& R) {
    R = A;
    if (!greater_eq(A, B)) return {0};
    vector<int> ans(A.size() - B.size() + 1);
    for (int i = A.size() - B.size(); i >= 0; i--)
        while (greater_eq(R, B, i)) {
            for (int j = 0; j < B.size(); j++) {
                if ((R[i + j] -= B[j]) < 0)
                    R[i + j] += 10, R[i + j + 1] -= 1;
            }
            ans[i]++;
        }
    return clear(ans), clear(R), ans;
}
```