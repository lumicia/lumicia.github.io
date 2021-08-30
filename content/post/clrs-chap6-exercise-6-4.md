---
title: "算法导论 Chap6 练习 6.4"
date: 2021-08-25T12:29:37+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: true
---

## 6.4-1

略。

## 6.4-2

初始化：子数组 $A[i+1..n]$ 为空，因此不变式正确。

保持：$A[1]$ 在 $A[1..i]$ 中最大，并且小于 $A[i+1..n]$ 中的元素。当把 $A[1]$ 放到第 $i$ 个位置，则 $A[i..n]$ 已经是排好序的。递减 `heap_size` 并调用 `MAX-HEAPIFY` 会将 $A[1..i-1]$ 变成最大堆。递减 $i$ 的值为下次迭代建立不变式。

终止：在循环结束时，$i = 1$。此时 $A[2..n]$ 是排好序的并且 $A[1]$ 是数组中最小的元素，这让整个数组 $A[1..n]$ 也是排好序的。

## 6.4-3

两种情况都是 $\Theta(n \lg n)$。

如果数组以升序排序，将数组转化成堆需要 $O(n)$ 时间。然后需要 $ n- 1$ 次调用过程 `MAX-HEAPIFY`，每次都执行 $\lg k$ 次操作。因此
$$
\sum_{k = 1}^{n - 1} \lg k = \lg((n - 1)!) = \Theta(n \lg n)
$$
降序同理。`BUILD-MAX-HEAP` 更快（常数因子），但整个计算时间由循环中的 `HEAPSORT` 主导，因此：
$$
\sum^n_{i = 1} \lg i = \lg n! = \Theta(n \lg n)
$$

## 6.4-4

当数组已经排好序时，需要线性时间将它转化成最大堆，然后需要 $n \lg n$ 时间来排序。

## 6.4-5 $\star$

参考：https://stackoverflow.com/questions/4589988/lower-bound-on-heapsort。

> The formal lower-bound proof is due to Schaffer and Sedgewick's"The  Analysis of Heapsort"paper. Here's a slightly paraphrased version of the proof that omits some of the technical details.
>
> To begin, let's suppose that $n = 2^k - 1$ for some $k$, which guarantees that we have a complete binary heap. I'll show how to handle this case separately later on. Because we have $2^k - 1$ elements, the first pass of heapsort will, in $\Theta(n)$, build up a heap of height $k$. Now, consider the first half of the dequeues from this heap, which removes $2^{k-1}$ nodes from the heap. The first key observation is that if you take the starting heap and then mark all of the nodes here that actually end up getting dequeued, they form a subtree of the heap (i.e. every node that get dequeued has a parent that also gets dequeued). You can see this because if this weren't the case, then there would be some node whose (larger) parent didn't get dequeued though the node itself was dequeued, meaning that the values are out of order.
>
> Now, consider how the nodes of this tree are distributed across the heap.  If you label the levels of the heap $0, 1, 2, \cdots, k - 1$, then there will be some number of these nodes in levels $0, 1, 2, \cdots, k - 2$  (that is, everything except the bottom level of the tree). In order for these nodes to get dequeued from the heap, then they have to get swapped up to the root, and they only get swapped up one level at a time. This means that one way to lower-bound the runtime of heapsort would be to count the number of swaps necessary to bring all of these values up to the root. In fact, that's exactly what we're going to do.
>
> The first question we need to answer is - how many of the largest $2^{k - 1}$ nodes are not in the bottom level of the heap? We can show that this is no greater than $2^{k - 2}$ by contradiction. Suppose that there are at least $2^{k - 2} + 1$ of the largest nodes in the bottom level of the heap. Then each of the parents of those nodes must also be large nodes in level $k - 2$. Even in the best case, this means that there must be at least $2^{k - 3} + 1$ large nodes in level $k - 2$, which then means that there would be at least $2^{k - 4} + 1$ large nodes in level $k - 3$, etc. Summing up over all of these nodes, we get that there are $2^{k - 2} + 2^{k - 3} + 2^{k - 4} + \cdots + 2^0 + k$ large nodes. But this value is strictly greater than $2^{k - 1}$, contradicting the fact that we're working with only $2^{k - 1}$ nodes here.
>
> Okay... we now know that there are at most $2^{k - 2}$ large nodes in the bottom layer. This means that there must be at least $2^{k - 2}$ of the large nodes in the first $k - 2$ layers. We now ask - what is the sum, over all of these nodes, of the distance from that node to the root? Well, if we have $2^{k - 2}$ nodes positioned somewhere in a complete heap, then at most $2^{k - 3}$ of them can be in the first $k - 3$ levels, and so there are at least $2^{k - 2} - 2^{k - 3} = 2^{k - 3}$ heavy nodes in level $k - 2$. Consequently, the total number of swaps that need to be performed are at least $(k - 2) 2^{k-3}$. Since $n = 2^k-1$, $k = \Theta(\lg n)$, and so this value is $\Theta(n \lg n)$ as required.
