---
title: "算法导论笔记 chap2 04：分治法与归并排序"
date: 2021-04-19T14:55:46+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: false
---

首先定义递归（recursive）算法：算法一次或多次递归地调用自身来解决密切相关的子问题。

一些问题可以分成相类似的规模较小的子问题，子问题也可以继续分解为更小的子问题。像这样递归地求解子问题，然后将解合并，从而得到原问题的解。这就是分治法（divide-and-conquer）。

分治法的三个步骤：

1. 分（Divide）：分解原问题为子问题；
2. 治（Conquer）：递归求解子问题；
3. 合（Combine）：合并子问题的解，得到原问题的解。

---

归并排序（merge sort）的操作：

1. 分：分解待排序的 $n$ 个元素的序列为两个子序列，每个都有 $n/2$ 个元素。
2. 治：使用归并排序递归地排序两个子序列。
3. 合：将两个已排序子序列合并得到完成排序的原序列。

**归并排序的关键是「合」这一步，将两个已排序的序列合并。**

用 `MERGE(A, p, q, r)` 来表示「合」过程。将两个已排序子数组 `A[p..q]` 和 `A[q + 1..r]` 合并。其中 $p\leqslant q<r$。

```
n1 = q - p + 1
n2 = r - q
let L[1..n1 + 1] and R[1..n2 + 1] be new arrays
for i = 1 to n1
	L[i] = A[p + i - 1]
for j = 1 to n2
	R[j] = A[q + j]
L[n1 + 1] = infinite
R[n2 + 1] = infinite
i = 1
j = 1
for k = p to r
	if L[i] <= R[j]
		A[k] = L[i]
		i = i + 1
	else A[k] = R[j]
		i = i + 1
```

`MERGE` 的运行时间是 $\Theta(n)$。

然后在归并排序主程序 `MERGE-SORT(A, p, r)` 中调用该过程：

```
if p < r
	q = ceil((p + r) / 2)
	MERGE-SORT(A, p, q)
	MERGE-SORT(A, q + 1, r)
	MERGE(A, p, q, r)
```

算法包含对自身的递归调用时，可以用递归等式（recurrence equation）或递归式（recurrence）来描述运行时间。

归并排序的最坏情况运行时间为 $\Theta(n\,\text{lg}\,n)$。