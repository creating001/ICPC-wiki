# 莫队

## 基础莫队

> 板子题网址: https://www.luogu.com.cn/problem/P1494

```cpp
struct Query {
    int id, l, r;
} q[N];
PII ans[N];
int n, m, len, a[N], cnts[N];

inline int get(int x) {
    return (x - 1) / len;
}

inline int calc(int x) {
    return x * (x - 1) / 2;
}

inline bool cmp(const Query& x, const Query& y) {
    int i = get(x.l), j = get(y.l);
    if (i != j) return i < j;
    return x.r < y.r;
}

inline void solve() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) cin >> a[i];
    len = int(sqrt(n));
    for (int i = 1; i <= m; i++) cin >> q[i].l >> q[i].r, q[i].id = i;
    sort(q + 1, q + 1 + m, cmp);
    for (int i = 1, j = 1, k = 0, sum = 0; i <= m; i++) {
        auto [id, l, r] = q[i];
        while (j < l)
            sum -= calc(cnts[a[j]]), cnts[a[j]]--, sum += calc(cnts[a[j]]), j++;
        while (j > l)
            j--, sum -= calc(cnts[a[j]]), cnts[a[j]]++, sum += calc(cnts[a[j]]);
        while (k < r)
            k++, sum -= calc(cnts[a[k]]), cnts[a[k]]++, sum += calc(cnts[a[k]]);
        while (k > r)
            sum -= calc(cnts[a[k]]), cnts[a[k]]--, sum += calc(cnts[a[k]]), k--;
        ans[id] = {sum, 1LL * (r - l + 1) * (r - l) / 2};
    }
    for (int i = 1; i <= m; i++) {
        auto [fz, fm] = ans[i];
        if (!fz || !fm) cout << "0/1" << '\n';
        else {
            LL d = __gcd(fz, fm);
            cout << fz / d << "/" << fm / d << '\n';
        }
    }
}
```

## 带修改莫队

> 板子题网址: https://www.luogu.com.cn/problem/P1903

```cpp
struct Query {
    int id, l, r, t;
} q[N];
struct Change {
    int p, x;
} c[N];
int n, m, mq, mc, len, a[N], cnts[M], ans[N];

inline int get(int x) {
    return x / len;
}

inline bool cmp(const Query& x, const Query& y) {
    int i = get(x.l), j = get(y.l);
    if (i != j) return i < j;
    i = get(x.r), j = get(y.r);
    if (i != j) return i < j;
    return x.t < y.t;
}

inline void solve() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) cin >> a[i];
    for (int i = 1; i <= m; i++) {
        char o;
        int l, r;
        cin >> o >> l >> r;
        if (o == 'Q') q[++mq] = {mq, l, r, mc};
        else c[++mc] = {l, r};
    }
    len = cbrt(1.0 * n * max(mc, 1)) + 1;
    sort(q + 1, q + 1 + mq, cmp);
    for (int i = 1, j = 1, k = 0, tt = 0, num = 0; i <= mq; i++) {
        auto [id, l, r, t] = q[i];
        while (j < l) num -= (--cnts[a[j++]]) == 0;
        while (j > l) num += (cnts[a[--j]]++) == 0;
        while (k < r) num += (cnts[a[++k]]++) == 0;
        while (k > r) num -= (--cnts[a[k--]]) == 0;
        while (tt < t) {
            tt++;
            if (c[tt].p >= l && c[tt].p <= r) {
                num -= (--cnts[a[c[tt].p]]) == 0;
                num += (cnts[c[tt].x]++) == 0;
            }
            swap(a[c[tt].p], c[tt].x);
        }
        while (tt > t) {
            if (c[tt].p >= l && c[tt].p <= r) {
                num -= (--cnts[a[c[tt].p]]) == 0;
                num += (cnts[c[tt].x]++) == 0;
            }
            swap(a[c[tt].p], c[tt].x);
            tt--;
        }
        ans[id] = num;
    }
    for (int i = 1; i <= mq; i++) cout << ans[i] << '\n';
}
```

## 回滚莫队

> 板子题网址: https://www.luogu.com.cn/problem/P5906

```cpp
struct Query {
    int id, l, r;
} q[N];
vector<int> nums;
int n, m, len, a[N], ans[N], L[N], R[N];

inline int get(int x) {
    return (x - 1) / len;
}

inline bool cmp(const Query& x, const Query& y) {
    int i = get(x.l), j = get(y.l);
    if (i != j) return i < j;
    return x.r < y.r;
}

inline void solve() {
    cin >> n;
    len = sqrt(n);
    for (int i = 1; i <= n; i++) cin >> a[i], nums.emplace_back(a[i]);

    sort(nums.begin(), nums.end());
    nums.erase(unique(nums.begin(), nums.end()), nums.end());
    for (int i = 1; i <= n; i++)
        a[i] = lower_bound(nums.begin(), nums.end(), a[i]) - nums.begin();

    cin >> m;
    for (int i = 1; i <= m; i++) cin >> q[i].l >> q[i].r, q[i].id = i;
    sort(q + 1, q + 1 + m, cmp);

    for (int x = 1; x <= m;) {
        int y = x;
        while (y <= m && get(q[x].l) == get(q[y].l)) y++;
        int right = get(q[x].l) * len + len - 1;

        while (x < y && q[x].r <= right) {
            auto [id, l, r] = q[x];
            int res = 0;
            static int pre[N];
            for (int i = l; i <= r; i++) pre[a[i]] = 0;
            for (int i = l; i <= r; i++)
                if (pre[a[i]]) res = max(res, i - pre[a[i]]);
                else pre[a[i]] = i;
            ans[id] = res, x++;
        }

        int res = 0, j = right + 1, k = right;
        while (x < y) {
            auto [id, l, r] = q[x];
            while (k < r) {
                k++;
                if (L[a[k]]) res = max(res, k - L[a[k]]);
                else L[a[k]] = k;
                R[a[k]] = k;
            }
            int backup = res;
            while (j > l) {
                j--;
                if (R[a[j]]) res = max(res, R[a[j]] - j);
                else R[a[j]] = j;
            }
            ans[id] = res;
            while (j < right + 1) {
                if (R[a[j]] == j) R[a[j]] = 0;
                j++;
            }
            res = backup, x++;
        }
        memset(L, 0, sizeof(L)), memset(R, 0, sizeof(R));
    }

    for (int i = 1; i <= m; i++) cout << ans[i] << '\n';
}
```

## 树上莫队

> 板子题网址: https://www.luogu.com.cn/problem/SP10707

```cpp
struct Query {
    int id, l, r, p;
} q[N];
vector<int> nums;
int h[N], e[N], nex[N], idx = 1;
int n, m, len, a[N], cnts[N], vis[N], ans[N];
int L[N], R[N], stk[N], top, dep[N], fa[N][S];

inline void add_edge(int x, int y) {
    e[idx] = y, nex[idx] = h[x], h[x] = idx++;
}

inline void dfs(int u, int p) {
    stk[++top] = u, L[u] = top;
    for (int i = h[u]; i; i = nex[i]) {
        int to = e[i];
        if (to == p) continue;
        dfs(to, u);
    }
    stk[++top] = u, R[u] = top;
}

inline void bfs(int u) {
    memset(dep, 0x3f, sizeof(dep));
    dep[0] = 0, dep[u] = 1;
    queue<int> que;
    que.emplace(u);
    while (!que.empty()) {
        int cur = que.front();
        que.pop();
        for (int i = h[cur]; i; i = nex[i]) {
            int to = e[i];
            if (dep[to] > dep[cur] + 1) {
                dep[to] = dep[cur] + 1;
                fa[to][0] = cur;
                for (int j = 1; j < S; j++)
                    fa[to][j] = fa[fa[to][j - 1]][j - 1];
                que.emplace(to);
            }
        }
    }
}

inline int lca(int x, int y) {
    if (dep[x] < dep[y]) swap(x, y);
    for (int i = S - 1; i >= 0; i--)
        if (dep[fa[x][i]] >= dep[y]) x = fa[x][i];
    if (x == y) return x;
    for (int i = S - 1; i >= 0; i--)
        if (fa[x][i] != fa[y][i]) x = fa[x][i], y = fa[y][i];
    return fa[x][0];
}

inline int get(int x) {
    return (x - 1) / len;
}

inline bool cmp(const Query& x, const Query& y) {
    int i = get(x.l), j = get(y.l);
    return i != j ? i < j : x.r < y.r;
}

inline void add(int x, int& res) {
    vis[x] ^= 1;
    if (!vis[x]) res -= (--cnts[a[x]]) == 0;
    else res += (cnts[a[x]]++) == 0;
}

inline void solve() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) cin >> a[i], nums.emplace_back(a[i]);

    sort(nums.begin(), nums.end());
    nums.erase(unique(nums.begin(), nums.end()), nums.end());
    for (int i = 1; i <= n; i++)
        a[i] = lower_bound(nums.begin(), nums.end(), a[i]) - nums.begin();

    for (int i = 1; i <= n - 1; i++) {
        int x, y;
        cin >> x >> y;
        add_edge(x, y), add_edge(y, x);
    }

    dfs(1, 1);
    bfs(1);

    for (int i = 1; i <= m; i++) {
        int x, y;
        cin >> x >> y;
        if (L[x] > L[y]) swap(x, y);
        int p = lca(x, y);
        if (p == x) q[i] = {i, L[x], L[y], 0};
        else q[i] = {i, R[x], L[y], p};
    }

    len = sqrt(top);
    sort(q + 1, q + 1 + m, cmp);

    for (int i = 1, j = 1, k = 0, res = 0; i <= m; i++) {
        auto [id, l, r, p] = q[i];
        while (j < l) add(stk[j++], res);
        while (j > l) add(stk[--j], res);
        while (k < r) add(stk[++k], res);
        while (k > r) add(stk[k--], res);
        if (p) add(p, res);
        ans[id] = res;
        if (p) add(p, res);
    }

    for (int i = 1; i <= m; i++) cout << ans[i] << '\n';
}
```
