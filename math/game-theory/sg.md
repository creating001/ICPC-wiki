# SG函数

> 板子题网址: https://www.acwing.com/problem/content/895
>
> 板子题网址: https://www.acwing.com/problem/content/896

## Mex运算

对于一个集合 $S$ ，其 $Mex$ 运算的结果为 $S$ 中不属于 $S$ 的最小非负整数。

$Mex(S)=min\{i|i\in N,i\notin S\}$

## SG函数

对于一个局面 $S$ ，其 $SG$ 函数的结果为 $S$ 的所有后继局面的 $SG$ 函数值的 $Mex$ 运算的结果。
其中终点的 $SG$ 函数值为 $0$ 。

$SG(S)=Mex\{SG(T)|T\in Next(S)\}$

## SG定理

有向图游戏的某个局面必胜，当且仅当该局面对应节点的 $SG$ 函数值大于0。
有向图游戏的某个局面必败，当且仅当该局面对应节点的 $SG$ 函数值等于0。

## 有向图游戏的和

设 $G1, G2, \dots, Gm$ 是 $m$ 个有向图游戏。定义有向图游戏 $G$ ，它的行动规则是任选某个有向图游戏Gi，并在Gi上行动一步。G被称为有向图游戏 $G1, G2, \dots, Gm$ 的和。

有向图游戏的和的 $SG$ 函数值等于它包含的各个子游戏 $SG$ 函数值的异或和，即：

$SG(G) = SG(G1) \oplus SG(G2) \oplus \dots \oplus SG(Gm)$

## SG函数的计算

```cpp
inline int sg(int x) {
    if (~f[x]) return f[x];
    // 记忆化搜索
    return f[x] = ans;
}
```
