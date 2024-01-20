# 前缀和与差分

## 基于 DP 计算高维前缀和
基于容斥原理来计算高维前缀和的方法，其优点在于形式较为简单，无需特别记忆，但当维数升高时，其复杂度较高。这里介绍一种基于 DP 计算高维前缀和的方法。该方法即通常语境中所称的 **高维前缀和**。

设高维空间 $U$ 共有 $D$ 维，需要对 $f[\cdot]$ 求高维前缀和 $\text{sum}[\cdot]$。令 $\text{sum}[i][\text{state}]$ 表示同 $\text{state}$ 后 $D - i$ 维相同的所有点对于 $\text{state}$ 点高维前缀和的贡献。由定义可知 $\text{sum}[0][\text{state}] = f[\text{state}]$，以及 $\text{sum}[\text{state}] = \text{sum}[D][\text{state}]$。

其递推关系为 $\text{sum}[i][\text{state}] = \text{sum}[i - 1][\text{state}] + \text{sum}[i][\text{state}']$，其中 $\text{state}'$ 为第 $i$ 维恰好比 $\text{state}$ 少 $1$ 的点。该方法的复杂度为 $O(D \times |U|)$，其中 $|U|$ 为高维空间 $U$ 的大小。

一种实现的伪代码如下：
$$
\begin{array}{ll}
\textbf{for } state \\
\qquad sum[state] \gets f[state] \\
\textbf{for } i \gets 0 \textbf{ to } D \\
\qquad \textbf{for } state' \textbf{ in } \text{lexicographical order} \\
\qquad \qquad sum[state] \gets sum[state] + sum[state']
\end{array}
$$

## 树上前缀和
设 $\textit{sum}_i$ 表示结点 $i$ 到根节点的权值总和。
然后：
-   若是点权，$x,y$ 路径上的和为 $\textit{sum}_x + \textit{sum}_y - \textit{sum}_\textit{lca} - \textit{sum}_{\textit{fa}_\textit{lca}}$。
-   若是边权，$x,y$ 路径上的和为 $\textit{sum}_x + \textit{sum}_y - 2\cdot\textit{sum}_{lca}$。

## 树上差分
树上差分可以理解为对树上的某一段路径进行差分操作，这里的路径可以类比一维数组的区间进行理解。例如在对树上的一些路径进行频繁操作，并且询问某条边或者某个点在经过操作后的值的时候，就可以运用树上差分思想了。

树上差分通常会结合 [树基础](../graph/tree-basic.md) 和 [最近公共祖先](../graph/lca.md) 来进行考察。树上差分又分为 **点差分** 与 **边差分**，在实现上会稍有不同。

## 例题
基于 DP 计算高维前缀和：
1. [CF 165E Compatible Numbers](https://codeforces.com/contest/165/problem/E)
2. [CF 383E Vowels](https://codeforces.com/problemset/problem/383/E)
3. [ARC 100C Or Plus Max](https://atcoder.jp/contests/arc100/tasks/arc100_c)

树上前缀和：
1. [LOJ 10134.Dis](https://loj.ac/problem/10134)
2. [LOJ 2491. 求和](https://loj.ac/problem/2491)

树上差分：
1. [洛谷 3128 最大流](https://www.luogu.com.cn/problem/P3128)
2. [JLOI2014 松鼠的新家](https://loj.ac/problem/2236)
3. [NOIP2015 运输计划](http://uoj.ac/problem/150)
4. [NOIP2016 天天爱跑步](http://uoj.ac/problem/261)
