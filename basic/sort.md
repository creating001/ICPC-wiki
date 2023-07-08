# 排序算法

> 板子题网址: https://www.luogu.com.cn/problem/P1177

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
    quick_sort(a, l, j), quick_sort(a, j + 1, r);
}
```

## 快速选择

> 板子题网址: https://www.luogu.com.cn/problem/P1923

时间复杂度: $O(N)$

```cpp
int quick_find(int* a, int l, int r, int k) {
    if (l >= r) return a[l];
    int x = a[rand() % (r - l) + l], i = l - 1, j = r + 1;
    while (i < j) {
        do i++; while (a[i] < x);
        do j--; while (a[j] > x);
        if (i < j) swap(a[i], a[j]);
    }
    if (k <= j) return quick_find(a, l, j, k);
    return quick_find(a, j + 1, r, k);
}
```

## 归并排序

> 应用: 求逆序对, 时间复杂度: $O(N \times \log N)$

```cpp
void merge_sort(int* a, int l, int r) {
    if (l >= r) return;
    int mid = (l + r) >> 1;
    merge_sort(a, l, mid), merge_sort(a, mid + 1, r);
    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (arr[i] < arr[j]) tmp[k++] = arr[i++];
        else tmp[k++] = arr[j++];
    while (i <= mid) tmp[k++] = arr[i++];
    while (j <= r) tmp[k++] = arr[j++];
    for (int index = l; index <= r; index++)
        a[index] = tmp[index - l];
}
```
