---
title: "算法导论 Chap6 练习 6.1"
date: 2021-08-22T13:10:05+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: false
---

## 6.1-1

元素个数最多 $2^{h + 1} - 1$，最少 $2^h$。

## 6.1-2

设该堆的高度为 $h$，则 $2^h \leqslant n \leqslant 2^{h + 1} - 1$。

得 $\lg(n + 1) - 1 \leqslant h \leqslant \lg n $。

故高度为 $\lfloor \lg n \rfloor$。

## 6.1-3

假设该子树中存在一个非根结点的最大元素，则它一定大于它的父结点，这违背了最大堆性质，因此这样的结点不存在。故子树中最大元素一定位于根结点。

## 6.1-4

假如最小元素所在结点有子结点，则违背最大堆性质，因此最小元素一定位于叶结点。

## 6.1-5

对于一个已排序的数组 $A$ 中的元素 $A[i]$，一定有 $A[\text{PARENT}(i)] \leqslant A[i]$，其中 $i > 1$，满足最小堆性质，所以 $A$ 是最小堆。

## 6.1-6

不是。$A[9] = 7$ 的父结点 $A[\text{PARENT}(9)] = 6$，不满足最大堆性质。

## 6.1-7

对于下标 $\lfloor n / 2 \rfloor + 1$，假设它有左子结点，则左子结点下标为
$$
\begin{aligned}
\text{LEFT}(\lfloor n / 2 \rfloor + 1) & = 2(\lfloor n / 2 \rfloor + 1) \\
& > 2(n / 2 + 1) - 2 \\
& = n + 2 - 2 \\
& = n
\end{aligned}
$$
下标为 $n$ 的结点一定是堆的最后一个叶结点，而该左子结点下标大于 $n$，因此不存在。故下标 $\lfloor n / 2 \rfloor + 1$ 是叶结点。从而下标大于 $\lfloor n / 2 \rfloor + 1$ 的结点也是叶结点。

对于下标 $\lfloor n / 2 \rfloor$，同理可推出 $n$ 为偶数时，它的左子结点下标是 $n$；$n$ 为奇数时，它的右子结点下标是 $n$，因此它不是叶结点。从而下标 $\lfloor n / 2 \rfloor + 1$ 是堆的第一个叶结点。

堆的叶结点总数为 $n - \lfloor n / 2 \rfloor = \lceil n / 2 \rceil$。
