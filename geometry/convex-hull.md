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
        ans += dist(stk[i], stk[i + 1]);
    }
    return ans;
}
```

## 三维凸包

> 板子题网址: https://www.luogu.com.cn/problem/P4724

```cpp
inline double rand_eps() {
    return ((double) rand() / RAND_MAX - 0.5) * EPS;
}

struct Point {
    double x, y, z;
    inline void shake() { x += rand_eps(), y += rand_eps(), z += rand_eps(); }
    inline double len() { return sqrt(x * x + y * y + z * z); }
} point[N];

inline Point operator-(Point a, Point b) {
    return {a.x - b.x, a.y - b.y, a.z - b.z};
}

inline double operator*(Point a, Point b) {
    return a.x * b.x + a.y * b.y + a.z * b.z;
}

inline Point operator^(Point a, Point b) {
    return {a.y * b.z - b.y * a.z, -a.x * b.z + b.x * a.z, a.x * b.y - b.x * a.y};
}

struct Plane {
    int v[3];
    inline Point norm() { return (point[v[1]] - point[v[0]]) ^ (point[v[2]] - point[v[0]]); }
    inline double area() { return norm().len() / 2; }
} plane[N], backup[N];

int n, m;
int g[N][N];

inline bool above(Plane a, Point b) {
    return (b - point[a.v[0]]) * a.norm() >= 0;
}

inline double convex_hull_3d() {
    plane[m++] = {0, 1, 2};
    plane[m++] = {2, 1, 0};
    for (int i = 3; i < n; i++) {
        int cnt = 0;
        for (int j = 0; j < m; j++) {
            bool t = above(plane[j], point[i]);
            if (!t) backup[cnt++] = plane[j];
            for (int k = 0; k < 3; k++) g[plane[j].v[k]][plane[j].v[(k + 1) % 3]] = t;
        }
        for (int j = 0; j < m; j++)
            for (int k = 0; k < 3; k++) {
                int a = plane[j].v[k], b = plane[j].v[(k + 1) % 3];
                if (g[a][b] && !g[b][a]) backup[cnt++] = {a, b, i};
            }
        m = cnt;
        for (int j = 0; j < m; j++) plane[j] = backup[j];
    }
    double ans = 0;
    for (int i = 0; i < m; i++) ans += plane[i].area();
    return ans;
}
```
