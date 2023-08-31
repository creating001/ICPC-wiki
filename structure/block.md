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

## 待修改莫队

## 回滚莫队

## 树上莫队

## 二次离线莫队
