# 分块及莫队

## 基础分块

> 板子题网址: https://www.luogu.com.cn/problem/P3372

```cpp
struct Block {
    LL add, sum;
} b[M];
int a[N], s[N], len;

inline void modify(int l, int r, int c) {
    if (s[l] == s[r]) {
        for (int i = l; i <= r; i++)
            a[i] += c, b[s[i]].sum += c;
    } else {
        int i = l, j = r;
        while (s[i] == s[l]) a[i] += c, b[s[i]].sum += c, i++;
        while (s[j] == s[r]) a[j] += c, b[s[j]].sum += c, j--;
        for (int k = s[i]; k <= s[j]; k++)
            b[k].sum += len * c, b[k].add += c;
    }
}

inline LL query(int l, int r) {
    if (s[l] == s[r]) {
        LL ans = 0;
        for (int i = l; i <= r; i++) ans += a[i] + b[s[i]].add;
        return ans;
    } else {
        LL ans = 0;
        int i = l, j = r;
        while (s[i] == s[l]) ans += a[i] + b[s[i]].add, i++;
        while (s[j] == s[r]) ans += a[j] + b[s[j]].add, j--;
        for (int k = s[i]; k <= s[j]; k++) ans += b[k].sum;
        return ans;
    }
}
```

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
        memset(L, 0, sizeof(L));
        memset(R, 0, sizeof(R));
    }

    for (int i = 1; i <= m; i++) cout << ans[i] << '\n';
}
```

## 树上莫队

## 二次离线莫队
