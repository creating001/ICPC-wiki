# 最短路算法

## Dijkstra算法

> Dijkstra算法用于解决非负权边的最短路问题。
>
> 板子题网址: https://www.luogu.com.cn/problem/P3371
>
> 板子题网址: https://www.luogu.com.cn/problem/P4779

### 朴素Dijkstra算法

时间复杂度为$O(V^2)$

```cpp

```

### 堆优化Dijkstra算法

时间复杂度为$O(ElogV)$。

```cpp

```

## Bellman-Ford算法

> Bellman-Ford算法用于解决负权边的最短路问题，其时间复杂度为$O(VE)$。
>
> 板子题网址: https://www.acwing.com/problem/content/855

```cpp

```

## SPFA算法

> SPFA算法是Bellman-Ford算法的队列优化版本，其时间复杂度为$O(kE)$ ($k$为最短路的平均长度)。
>
> 板子题网址: https://www.acwing.com/problem/content/853
> 板子题网址: https://www.luogu.com.cn/problem/P3385

```cpp

```


## Floyd算法

> Floyd算法用于解决稠密图的最短路问题，其时间复杂度为$O(V^3)$。
>
> 板子题网址: https://www.luogu.com.cn/problem/B3647

```cpp

```

## Johnson算法

> Johnson算法用于解决稀疏图的最短路问题，其时间复杂度为$O(V^2logV+VE)$。
>
> 板子题网址: https://www.luogu.com.cn/problem/P5905

```cpp

```
