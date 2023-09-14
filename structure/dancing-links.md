# 舞蹈链(DLX)

## 精确覆盖问题

> 板子题网址: https://www.luogu.com.cn/problem/P4929

```cpp
#include<bits/stdc++.h>

using namespace std;

const int N = 6e3 + 5;

int n, m, ans[N], top;
int L[N], R[N], U[N], D[N], S[N], row[N], col[N], idx;

inline void init() {
    for (int i = 0; i <= m; i++)
        L[i] = i - 1, R[i] = i + 1, U[i] = D[i] = i;
    L[0] = m, R[m] = 0, idx = m + 1;
}

inline void add(int& hh, int& tt, int r, int c) {
    row[idx] = r, col[idx] = c, S[c]++;
    U[idx] = c, D[idx] = D[c], U[D[c]] = idx, D[c] = idx;
    R[hh] = L[tt] = idx, R[idx] = tt, L[idx] = hh;
    tt = idx++;
}

inline void remove(int p) {
    R[L[p]] = R[p], L[R[p]] = L[p];
    for (int i = D[p]; i != p; i = D[i]) {
        for (int j = R[i]; j != i; j = R[j]) {
            S[col[j]]--;
            U[D[j]] = U[j], D[U[j]] = D[j];
        }
    }
}

inline void resume(int p) {
    for (int i = U[p]; i != p; i = U[i]) {
        for (int j = L[i]; j != i; j = L[j]) {
            U[D[j]] = j, D[U[j]] = j;
            S[col[j]]++;
        }
    }
    R[L[p]] = p, L[R[p]] = p;
}

inline bool dfs() {
    if (!R[0]) return true;
    int p = R[0];
    for (int i = R[0]; i; i = R[i])
        if (S[i] < S[p]) p = i;
    remove(p);
    for (int i = D[p]; i != p; i = D[i]) {
        ans[top++] = row[i];
        for (int j = R[i]; j != i; j = R[j]) remove(col[j]);
        if (dfs()) return true;
        for (int j = L[i]; j != i; j = L[j]) resume(col[j]);
        top--;
    }
    resume(p);
    return false;
}

inline void solve() {
    cin >> n >> m;
    init();
    for (int i = 1; i <= n; i++) {
        int hh = idx, tt = idx;
        for (int j = 1; j <= m; j++) {
            int x;
            cin >> x;
            if (x) add(hh, tt, i, j);
        }
    }
    if (dfs())
        for (int i = 0; i < top; i++) cout << ans[i] << " \n"[i == top - 1];
    else
        cout << "No Solution!" << '\n';
}
```
