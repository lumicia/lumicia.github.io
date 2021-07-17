---
title: "算法导论 Chap2 练习题 2.3"
date: 2021-04-25T17:35:58+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: false
---

## 2.3-1

> Using Figure 2.4 as a model, illustrate the operation of merge sort on the array
> $A = \langle 3, 41, 52, 26, 38, 57, 9, 49\rangle$.

<!--more-->

略。

## 2.3-2

> Rewrite the $\text{MERGE}$ procedure so that it does not use sentinels, instead stopping once either array $L$ or $R$ has had all its elements copied back to $A$ and then copying the remainder of the other array back into $A$.

```
n1 = q - p + 1
n2 = r - q
let L[1..n1] and R[1..n2] be new arrays
for i = 1 to n1
	L[i] = A[p + i - 1]
for j = 1 to n2
	R[j] = A[q + j]
i = 1
j = 1
k = p
while i < n1 and j < n2
    if L[i] < R[j]
        A[k] = L[i]
        i = i + 1
    else
        A[k] = R[j]
        j = j + 1
    k = k + 1
if j == n2
    A[k..r +] = L[i..n1]
```

## 2.3-3

> Use mathematical induction to show that when n is an exact power of 2, the solution of the recurrence
> $$
> T(n)=\begin{cases}
> 2 & \text{if}\ n=2,\\\\
> 2T(n/2)+n & \text{if}\ n=2^k,\ \text{for}\ k>1
> \end{cases}
> $$
> is $T(n)=n\lg n$.

基本情况：当 $n=2$ 时，$T(n)=2\ \lg 2 = 2$。

设 $n=2^k$，$T(n)=n\lg n=2^k\lg 2^k=2^k\cdot k$。

对于 $n=2^{k+1}$，
$$
\begin{aligned}
T(n) & = 2 \cdot T(2^{k + 1}/2)+2^{k + 1} \\\\
     & = 2 \cdot T(2^k) + 2^{k + 1} \\\\
     & = 2 \cdot 2^k + 2^{k + 1} \\\\
     & = 2^{k + 1}\cdot (k + 1) \\\\
     & = 2^{k + 1}\cdot \lg 2^{k + 1} \\\\
     & = n\lg n.
\end{aligned}
$$

故 $T(n)=n\lg n$，其中 $n$ 是 2 的整数幂。

## 2.3-4

> We can express insertion sort as a recursive procedure as follows. In order to sort $A[1..n]$, we recursively sort $A[1..n - 1]$ and then insert $A[n]$ into the sorted array $A[1..n - 1]$. Write a recurrence for the running time of this recursive version of insertion sort.

最坏情况下将 $A[n]$ 插入到已排序数组 $A[1..n - 1]$ 需要的时间为 $\Theta(n)$，因此
$$
T(n) =
\begin{cases}
\Theta(1) &\ \text{if}\ n = 1, \\\\
T(n - 1) + \Theta(n) &\ \text{if}\ n>1.
\end{cases}
$$
递推式的结果是 $\Theta(n^2)$。

## 2.3-5

> Referring back to the searching problem (see Exercise 2.1-3), observe that if the sequence $A$ is sorted, we can check the midpoint of the sequence against $v$ and eliminate half of the sequence from further consideration. The **_binary search_** algorithm repeats this procedure, halving the size of the remaining portion of the sequence each time. Write pseudocode, either iterative or recursive, for binary search. Argue that the worst-case running time of binary search is $\Theta(\lg n)$.

迭代式：`ITERATIVE-BINARY-SEARCH(A, v, lo, hi)`

```
while lo < hi
    mid = floor((lo + hi) / 2)
    if v == A[mid]
        return mid
    else if v > A[mid]
        lo = mid + 1
    else
        hi = mid - 1
return NIL
```

递归式：`RECURSIVE-BINARY-SEARCH(A, v, lo, hi)`

```
if lo > hi
    return NIL
    mid = floor((lo + hi) / 2)
    if v == A[mid]
        return mid
    else if v > A[mid]
        return RECURSIVE-BINARY-SEARCH(A, v, mi + 1, hi)
    else
        return RECURSIVE-BINARY-SEARCH(A, v, lo, mid - 1)
```

算法每次都将 $v$ 和数组中点元素相比较，从而将搜索范围减半。

递归式：
$$
T(n) =
\begin{cases}
\Theta (1) & \text{if } n = 1, \\\\
T(n/2)+\Theta (1) & \text{if } n > 1.
\end{cases}
$$
递归式的结果是 $\Theta(n\lg n)$。

## 2.3-6

> Observe that the **while** loop of lines 5–7 of the $\text{INSERTION-SORT}$ procedure in Section 2.1 uses a linear search to scan (backward) through the sorted subarray $A[1..j - 1]$. Can we use a binary search (see Exercise 2.3-5) instead to improve the overall worst-case running time of insertion sort to $\Theta(n\lg n)$?

不能。查找虽然是 $\Theta(\lg n)$，但移动元素仍然是 $\Theta(n)$。

## 2.3-7 $\star$

> Describe a $\Theta(n\lg n)$-time algorithm that, given a set $S$ of $n$ integers and another integer $x$, determines whether or not there exist two elements in $S$ whose sum is exactly $x$.

首先对 $S$ 排序，需要 $\Theta(n\lg n)$ 时间。

然后对于 $S$ 中的元素 $e_i$，$i = 1,\dots,n$，用二分查找在 $A[i + 1..n]$ 中查找 $e_j = x - e_i$，需要 $\Theta(\lg n)$ 时间。

如果找到 $e_j$，返回 $j$ 的值；否则进行下一次迭代。

算法总共所需时间 $\Theta(n\lg n) + n\cdot \Theta(\lg n)=\Theta(n\lg n)$。