---
title: "算法导论 Chap2 练习题 2.2"
date: 2021-04-17T11:21:42+08:00
categories: ["Algorithms"]
tags: ["算法导论"]
draft: false
---

> 2.2-1
> Express the function $n^3/1000-100n^2-100n+3$ in terms of $\Theta$-notation.

$\Theta(n^3)$。

> 2.2-2
> Consider sorting $n$ numbers stored in array $A$ by first finding the smallest element of $A$ and exchanging it with the element in $A[1]$. Then find the second smallest element of $A$, and exchange it with $A[2]$. Continue in this manner for the first $n-1$ elements of $A$. Write pseudocode for this algorithm, which is known as **_selection sort_**. What loop invariant does this algorithm maintain? Why does it need to run for only the first $n-1$ elements, rather than for all $n$ elements? Give the best-case
> and worst-case running times of selection sort in $\Theta$-notation.

```
for i = 1 to A.length - 1
	min = A[i]
	for j = i + 1 to A.length
		if A[i] > A[j]
			min = A[j]
	swap(A[i], min)
```

1、循环不变式：在 `for` 循环的开始，子数组 $A[1..i-1]$ 由数组 $A$ 中已排序的最小的 $i-1$ 个元素组成。

**Initialization**：循环开始前 $i=1$，子数组为空，因此为真。

**Maintenance**：循环每次迭代结束时，将最小值放到 $A[i]$ 的位置，子数组 $A[1..i-1]$ 中的元素都比 $A[i]$ 小，且已排序，因此循环不变式为真。在循环下一次迭代中递增 $i$ 的值将维持循环不变式。

**Termination**：循环结束的条件为 $i=A.length=n$。此时子数组 $A[1..n-1]$ 由原来数组 $A[1..n]$ 的 $n-1$ 个元素组成，且已排序。而剩下的元素则保证不小于子数组中的元素，因此整个数组都是有序的。故算法正确。

2、前 $n-1$ 个元素排序完成后，子数组 $A[1..n-1]$ 由数组 $A$ 中已排序的最小的 $n-1$ 个元素组成，因此第 $n$ 个元素必定是数组中最大的元素，此时它已经在正确的位置，无需再排序。

3、选择排序最好的情况（数组已排序）和最坏的情况（数组逆序）都需要将外循环和内循环的每一条语句执行一遍，所以都需要 $\Theta(n^2)$ 时间。

> 2.2-3
> Consider linear search again (see Exercise 2.1-3). How many elements of the input sequence need to be checked on the average, assuming that the element being searched for is equally likely to be any element in the array? How about in the worst case? What are the average-case and worst-case running times of linear search in $\Theta$-notation? Justify your answers.

1、分析在数组 $A[a_1,a_2,\dots,a_n]$ 中进行线性查找的平均情况，第 1 个元素需要查找 1 个元素，第 2 个元素查找 2 个，以此类推，第 n 个元素查找 n 个。总共查找 n 次，因此平均个数为 $\cfrac{1+2+\dots+n}n=\cfrac{n+1}2$ 个。

2、最坏情况是查找到第 $n$ 个元素。

3、平均情况和最坏情况的运行时间都是 $\Theta(n)$。

> 2.2-4
> How can we modify almost any algorithm to have a good best-case running time? 

没看懂问题 (￣﹏￣；)，看了下答案，应该是通过添加一个特殊的情况，如果输入符合这个特例，则输出预先计算的结果，这样就可以得到最好情况的运行时间了。