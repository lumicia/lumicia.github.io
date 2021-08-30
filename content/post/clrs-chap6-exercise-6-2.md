---
title: "算法导论 Chap6 练习 6.2"
date: 2021-08-23T11:13:12+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: false
---

## 6.2-1

略。

## 6.2-2

`MIN-HEAPIFY(A, i)`

```
l = LEFT(i)
r = RIGHT(i)
if l <= A.heap_size and A[l] < A[i]
	smallest = l
else smallest = i
if r <= A.heap_size and A[r] < A[smallest]
	smallest = r
if smallest != r
	exchange A[i] with A[smallest]
	MIN-HEAPIFY(A, smallest)
```

运行时间和 `MAX-HEAPIFY` 相同，也是 $O(\lg n)$。

## 6.2-3

没有影响，函数直接返回。

## 6.2-4

此时，该结点是叶结点，因此没有影响，函数直接返回。

## 6.2-5

`MAX-HEAPIFY-ITERATIVE(A, i)`

```
while true
    l = LEFT(i)
    r = RIGHT(i)
    if l <= A.heap_size and A[l] > A[i]
    	largest = l
    else largest = i
    if r <= A.heap_size and A[r] > A[largest]
    	largest = r
    if largest == i
    	return
    exchange A[i] with A[largest]
    i = largest
```

## 6.2-6

最坏情况是从根结点一直递归到叶结点，因为堆的高度为 $\lfloor \lg n \rfloor$，所以最坏情况运行时间为 $\Omega(\lg n)$。
