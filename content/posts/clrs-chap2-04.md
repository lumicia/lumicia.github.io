---
title: "算法导论 chap2-4：分治法与归并排序"
date: 2021-03-29T14:55:46+08:00
categories: ["Algorithm"]
tags: ["CLRS", "Python"]
draft: false
---

首先定义递归（recursive）算法：算法一次或多次递归地调用自身来解决密切相关的子问题。

分治法（divide-and-conquer）：一些问题可以分成相类似的规模较小的子问题，子问题也可以继续分解为更小的子问题。像这样递归地求解子问题，然后将解合并，从而得到原问题的解。

<!--more-->

分治法的三个步骤：

1. 分解（Divide）：分解原问题为子问题；
2. 解决（Conquer）：递归求解子问题，对规模足够小的子问题直接求解；
3. 合并（Combine）：合并子问题的解，得到原问题的解。

归并排序（merge sort）的操作：

1. 分解：将待排序的 $n$ 个元素的序列分解为两个子序列，每个都有 $n/2$ 个元素。
2. 解决：使用归并排序递归地排序两个子序列。
3. 合并：将两个已排序子序列合并得到完成排序的原序列。

归并排序的关键是「合」这一步，将两个已排序的序列合并。用辅助函数 `merge` 表示这个过程。

```python
def merge(a: list[int | float], p: int, q: int, r: int) -> None:
    left = a[p : q + 1]
    right = a[q + 1 : r + 1]
    left.append(float('inf'))
    right.append(float('inf'))
    i = j = 0
    
    for k in range(p, r + 1):
        if left[i] <= right[j]:
            a[k] = left[i]
            i += 1
        else:
            a[k] = right[j]
            j += 1
```

数组 `a[p:r]` 的子数组 `a[p : q + 1]` 和 `a[q + 1 : r + 1]` 都已经完成排序，现在需要将它们合并，使 `a[p:r]` 有序。其中数组下标 `p`、`q` 和 `r` 满足 $p\leqslant q < r$。

`merge` 首先将两个子数组复制到 `left` 和 `right` 数组中，然后分别添加  `float('inf')` 到 `left` 和 `right` 的末尾，用无限作为哨兵。然后在 `for` 循环中比较 `left` 和 `right` 中元素的大小，按序复制回数组 `a[p:r]` 中。`merge` 需要 $\Theta(n)$ 时间，其中 $n = r - p + 1$，表示合并元素的总数。

现在可以将归并排序的三个步骤用主函数 `merge_sort` 来表示了：

```python
def merge_sort(a: list[int | float], p: int, r: int) -> None:
    if p < r:
        q = (p + r) // 2
        merge_sort(a, p, q)
        merge_sort(a, q + 1, r)
        merge(a, p, q, r)
```

对于子数组的首尾元素的下标，若 $p \geqslant r$，则子数组中只有一个元素，子数组已排序，这是算法的基本情况。其余情况下需要将子数组分解为两个更小的子数组，递归调用 `merge_sort` 直到基本情况。根据 $p$ 和 $r$ 的奇偶情况分析可得子数组 `a[p : q + 1]` 包含 $\lceil n / 2 \rceil$ 个元素，子数组  `a[q + 1 : r + 1]` 包含 $\lfloor n / 2 \rfloor$ 个元素。

然后从基本情况开始合并，一对子数组合并为包含两个子数组所有元素的子数组，接着与类似的子数组合并为更大的子数组，以此类推，直到合并为长度为 $n$ 的数组。

算法包含对自身的递归调用时，可以用递归等式（recurrence equation）或递归式（recurrence）来描述运行时间。通过主定理可以证明归并排序的最坏情况运行时间为 $\Theta(n\ \text{lg}\ n)$。

为了方便起见，将练习 2.3-2 中未使用哨兵的实现也列出。

```python
def merge(a: list[int], p: int, q: int, r: int) -> None:
    left = a[p : q + 1]
    right = a[q + 1 : r + 1]
    i = j = 0
    k = p

    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            a[k] = left[i]
            i += 1
        else:
            a[k] = right[j]
            j += 1
        k += 1

    if j == len(right):
        a[k : r + 1] = left[i:]

def merge_sort(a: list[int], p: int, r: int) :
    if p < r:
        q = (p + r) // 2
        merge_sort(a, p, q)
        merge_sort(a, q + 1, r)
        merge(a, p, q, r)
```

