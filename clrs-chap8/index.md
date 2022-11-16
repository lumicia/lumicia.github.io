# 算法导论 Chap8：线性时间排序


如果排序算法的最终结果中，各元素的次序依赖于它们之间的比较，则把这种排序算法称为**比较排序**。我们对比较排序算法需要比较的最少次数感兴趣，即算法的下界。

为方便起见，我们对算法的输入序列 $<a_1, a_2, \cdots, a_n>$ 做出两个假设：
- 所有输入的元素互异。
- 元素比较只采用 $a_i \leqslant a_j$ 的形式。其中 $1 \leqslant i$，$j \leqslant n$。

我们把比较排序抽象为一棵决策树。决策树是一棵满二叉树（full binary tree，中文版在此处有错误）。在满二叉树中，除了叶结点以外的结点都有两个子结点。决策树可用表示在给定输入规模下，某一特定排序算法对所有元素的比较操作。

决策树中的内部结点用 $i:j$ 标注，表示一次比较 $a_i \leqslant a_j$ ；叶结点用序列元素的一个随机排列标注，表示 $n!$ 中排列中的一种。排序算法的执行对应于一条从根结点到叶结点的路径。如果  $a_i \leqslant a_j$ 则前往左子树，否则前往右子树。当到达一个叶结点时，排序算法确定序列的顺序。在正确的排序算法中，每个叶结点都可以通过从根结点开始的某条路径到达，该路径就是算法的一次实际执行过程。

当比较排序算法出现最坏情况时所需的比较次数，就是在决策树中从根结点到任意可达叶结点最长简单路径的长度，即决策树的高度。当决策树中每种排列的叶结点都是可达的，决策树高度的下界就是比较排序算法运行时间的下界。

>定理：在最坏情况下，任何比较排序算法都需要 $\Omega(n \lg n)$ 次比较。
>
>推论：堆排序和归并排序都渐近最优的比较排序算法。

接下来介绍三种使用运算而非比较的排序算法，它们都有线性时间的复杂度。

## 计数排序

计数排序假设 $n$ 个输入元素都是处于 $0$ 到 $k$ 之间的整数。其中 $k$ 是一个整数，当 $k = O(n)$ 时，计数排序的运行时间为 $\Theta(n)$。

计数排序的基本思想是对每个输入元素 $x$，确定小于 $x$ 的元素个数，从而把 $x$ 放到输出数组中的正确位置。

`count_sort`：

```python
def count_sort(a: list[int], b: list[int], k: int):
    c = [0] * (k + 1)

    for j in a:
        c[j] += 1

    for i in range(1, k + 1):
        c[i] += c[i - 1]

    for j in a[::-1]:
        b[c[j] - 1] = j
        c[j] -= 1
```

函数参数 `a` 表示输入数组，`b` 表示输出数组。变量 `c` 是辅助数组，下标从 $0$ 开始，长度为 $k + 1$。这样就可以用 `c` 的下标表示 $0$ 到 $k$ 的整数。

`c` 的所有元素都初始化为 $0$，然后第一个 `for` 循环对 `a` 中元素计数，循环结束后 `c` 中的数组元素表示 `a` 中每个整数出现的次数。第二个 `for` 循环计数所有小于等于 `i` 的元素个数。第三个 `for` 循环将 `a` 中所有元素放在 `b` 中正确排序后的位置。

计数排序是稳定排序，具有相同值的元素在输入和输出数组中的相对次序不变。

## 基数排序

基数排序利用一种稳定排序在线性时间内对最大 $d$ 位整数数组排序。

步骤如下：
1. 取得最大数，以它的数位作为基准，给其他数位较短的数的左端补零。
2. 对从最低有效位开始，到最高有效位的每一位数字排序。

第二步使用稳定排序算法来排序，如计数排序。其中一种方法是用桶分配和收集数位。我们使用十进制，因此使用 10 个桶，从 0 到 9 分别编号。将每个数字的个位按值分配到对应的桶中。然后将桶中的数字按序收集到辅助数组中。对每个数位进行分配和收集，最终得到完成排序的数组。

```python
def radix_sort(a: list[int]) -> list[int]:
    max_num = max(a)
    num_digits = len(str(max_num))

    for i in range(num_digits):
		buckets = [[] for _ in range(10)]
        for num in a:
            digit = (num // 10 ** i) % 10
            buckets[digit].append(num)
	        a = [k for bucket in buckets for k in bucket]

    return a
```

基数排序需要分配的桶的数量与使用的进制的基数有关，设为 $k$。若数组长度为 $n$，则含有 $d$ 位数字数组每一位所需排序时间是 $O(n + r)$，因此总共所需时间为 $O((n + k) d)$。$d$ 和 $k$ 都是常数，因此基数排序的运行时间为 $O(n)$。所需空间最多为 $O(kn)$。

## 桶排序

桶排序假设输入数据服从均匀分布，平均情况下运行时间为 $O(n)$。

```python
def bucket_sort(a: list[float]) -> list[float]:
    n = len(a)
    b = [[]] * n

    for i in range(n):
        b[i] = []

    for i in range(1, n + 1):
        b[int(a[i - 1] * n)].append(a[i - 1])

    for i in range(n):
        insertion_sort(b[i])

    b = [i for j in b for i in j]
    return b
```

其中涉及 Python 数组扁平化[^1]。常用方法有两层循环的列表推导、用 `sum` 合并到一个空数组、用 `+=` 运算符合并每个子数组到一个空数组等。

桶排序同样属于稳定排序。

## 排序的稳定性

我们已经学习了 9 种排序方法，其中选择排序、堆排序、快速排序都是不稳定的（还有一种不稳定的排序是第二章章末注记中提到的希尔排序）排序方法，其他都是稳定的。那么我们为什么要区分稳定排序和不稳定排序呢？

答案是在实际应用中可能需要对同一组数据根据不同的关键字多次排序，使用稳定排序可以在多次排序后得到符合需求的数据排列方式。比如我们需要对

[^1]: https://stackoverflow.com/questions/952914/