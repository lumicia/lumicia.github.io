---
title: "算法导论笔记 chap2 04：分治法与归并排序"
date: 2021-04-19T14:55:46+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: false
---

首先定义递归（recursive）算法：算法一次或多次递归地调用自身来解决密切相关的子问题。

一些问题可以分成相类似的规模较小的子问题，子问题也可以继续分解为更小的子问题。像这样递归地求解子问题，然后将解合并，从而得到原问题的解。这就是分治法（divide-and-conquer）。

<!--more-->

分治法的三个步骤：

1. 分解（Divide）：分解原问题为子问题；
2. 解决（Conquer）：递归求解子问题；
3. 合并（Combine）：合并子问题的解，得到原问题的解。

---

归并排序（merge sort）的操作：

1. 分解：分解待排序的 $n$ 个元素的序列为两个子序列，每个都有 $n/2$ 个元素。
2. 解决：使用归并排序递归地排序两个子序列。
3. 合并：将两个已排序子序列合并得到完成排序的原序列。

**归并排序的关键是「合」这一步，将两个已排序的序列合并。**

用 `MERGE(A, p, q, r)` 来表示「合并」过程。将两个已排序的子数组 `A[p..q]` 和 `A[q + 1..r]` 合并。其中 $p\leqslant q < r$。`MERGE` 的运行时间是 $\Theta(n)$。

`MERGE(A, p, q, r)`：

```
n1 = q - p + 1
n2 = r - q
let L[1..n1 + 1] and R[1..n2 + 1] be new arrays
for i = 1 to n1
    L[i] = A[p + i - 1]
for j = 1 to n2
    R[j] = A[q + j]
L[n1 + 1] = INFINITE
R[n2 + 1] = INFINITE
i = 1
j = 1
for k = p to r
    if L[i] <= R[j]
        A[k] = L[i]
        i = i + 1
    else A[k] = R[j]
        j = j + 1
```

第 1-2 行 `n1` 和 `n2` 分别表示子数组 `A[p..q]` 和 `A[q + 1..r]` 的长度。

第 3 行因为 `L` 和 `R` 都加入一个特殊的哨兵作为数组尾元素来指示子数组为空，所以 `L` 和 `R` 长度为 `n1 + 1` 和 `n2 + 1`。

第 4-7 行将 `A[p..q]` 和 `A[q + 1..r]` 的元素依次拷贝到 `L` 和 `R` 中。

第 8-9 行分别设置 `L` 和 `R` 的尾元素为值为 $\infin$（`INFINITE`）的哨兵。

第 10-11 行分别设置两个标志，起始指向子数组的首元素。

第 12-17 行按序将两个子数组的元素拷贝到合并数组，并移动相应子数组的标志。

然后在归并排序主过程 `MERGE-SORT(A, p, r)` 中调用该过程。

`MERGE-SORT(A, p, r)`：

```
if p < r
    q = floor((p + r) / 2)
    MERGE-SORT(A, p, q)
    MERGE-SORT(A, q + 1, r)
    MERGE(A, p, q, r)
```

对 `A[1..n]` 第一次执行归并排序时，调用 `MERGE-SORT(A, 1, A.length)`。

如果有 `p >= r`，则该子数组只有一个元素，因此已排序。

考虑到数组长度为奇数的情况，此时不能均分数组，让子数组 `A[p..q]` 包含 $\lceil n / 2\rceil$ 个元素，`A[q + 1..r]` 包含 $\lfloor n / 2 \rfloor$ 个元素。

算法包含对自身的递归调用时，可以用递归等式（recurrence equation）或递归式（recurrence）来描述运行时间。

归并排序的最坏情况运行时间为 $\Theta(n\ \text{lg}\ n)$。

下面的实现采用了练习 2.3-2 中的方式，没有使用哨兵。

Python 实现：

```python
def merge(a: list[int], p: int, q: int, r: int):
    left = a[p: q + 1]
    right = a[q + 1: r + 1]
    i = j = 0
    k = p

    while i < len(left) and j < len(right):
        if L[i] <= R[j]:
            a[k] = L[i]
            i += 1
        else:
            a[k] = R[j]
            j += 1
        k += 1

    if j == len(right):
        a[k:r + 1] = L[i:]

def merge_sort(a: list[int], p: int, r: int):
    if p < r:
        q = (p + r) // 2
        merge_sort(a, p, q)
        merge_sort(a, q + 1, r)
        merge(a, p, q, r)
```

