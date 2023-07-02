# 排序算法

> 板子题网址1: https://www.luogu.com.cn/problem/P1177
>
> 板子题网址2: https://leetcode.cn/problems/sort-an-array/

## 快速排序

时间复杂度: $O(N \times \log N)$

```cpp
void quick_sort(int* a, int l, int r) {
    if (l >= r) return;
    int i = l - 1, j = r + 1;
    int x = a[rand() % (r - l + 1) + l];
    while (i < j) {
        do i++; while (a[i] < x);
        do j--; while (a[j] > x);
        if (i < j) swap(a[i], a[j]);
    }
    quick_sort(a, l, j);
    quick_sort(a, j + 1, r);
}
```
