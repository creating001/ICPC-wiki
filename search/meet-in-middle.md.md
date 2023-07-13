# 折半搜索

> 板子题网址: https://www.luogu.com.cn/problem/P4799
>
> 板子题网址: https://www.luogu.com.cn/problem/P9234

`Meet in the middle` 算法没有正式译名，常见的翻译为「折半搜索」、「双向搜索」或「中途相遇」。
它适用于输入数据较小，但还没小到能直接使用暴力搜索的情况。

`Meet in the middle` 算法的主要思想是将整个搜索过程分成两半，分别搜索，最后将两半的结果合并。

暴力搜索的复杂度往往是指数级的，而改用 `meet in the middle` 算法后复杂度的指数可以减半，即让复杂度从 $O(a^b)$ 降到 $O(a^{b/2})$。
