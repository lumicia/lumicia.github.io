---
title: "算法导论 Chap6 练习 6.5"
date: 2021-08-25T15:28:33+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: true
---

## 6.5-1

略。

## 6.5-2

略。

## 6.5-3

`HEAP-MINIMUM(A)`

```
return A[1]
```

`HEAP-EXTRACT-MIN(A)`

```
if A.heap_size < 1
	error "heap underflow"
min = A[1]
A[1] = A[A.heap_size]
A.heap_size = A.heap_size - 1
MIN-HEAPIFY(A, 1)
return min
```

`HEAP-DECREASE-KEY(A, i, key)`

```
if key > A[i]
	error "new key is larger than current key"
A[i] = key
while i > 1 and A[PARENT(i)] > A[i]
	exchange A[i] with A[PARENT]
	i = PARENT(i)
```

`MIN-HEAP-INSER(A, key)`

```
A.heap_size = A.heap_size + 1
A[A.heap_size] = Infinity
HEAP-DECRASE-KEY(A, A.heap_size, key)
```

## 6.5-4

为了维持 `HEAP-INCREASE-KEY` 过程的 `if` 语句的条件为真。

## 6.5-5

初始化：`A[1..heap_size]` 满足最大堆性质，除非 $A[i]$ 大于 $A[PARENT(i)]$，因为 $A[i]$ 被修改了。$A[i]$ 大于它的子结点，否则 `if` 语句会失败，不会进入 `while` 循环。

保持：在循环中交换 $A[i]$ 和 $A[PARENT(i)]$，满足了最大堆性质。然后在循环下次迭代继续和父结点的父结点比较，继续进行交换，以继续维持最大堆性质。

终止：当堆中元素遍历完，或 $A[i]$ 小于父结点时（维持了最大堆性质），结束循环。此时，$A$ 是最大堆。

## 6.5-6

`HEAP-INCREASE-KEY(A, i, key)`

```
if key <A[i]
	error "new key is smaller than current key"
while i > 1 and A[PARENT(I)] < key
	A[i] = A[PARENT(i)]
	i = PARENT(i)
A[i] = key
```

## 6.5-7

对于队列，优先级队列每次插入新元素时递减所添加元素的优先级。

对于栈，优先级队列则以递增优先级的方式添加元素。

## 6.5-8

`HEAP-DELETE(A, i)`

```
if A[i] > A[A.heap-size]
	A[i] = A[A.heap_size]
	MAX-HEAPIFY(A, i)
else HEAP-INCREASE-KEY(A, i, A[A.heap_size])
A.heap_size = A.heap_size - 1
```

## 6.5-9

从 $k$ 个有序链表中分别从头取一个元素，然后放到一个最小堆实现的最小优先级队列中。

合并时，从最小堆中取出最小的元素，插入该元素对应链表中的下一个元素到最小堆中。如果链表为空，则从最小堆中取出倒数第二小的元素，以此类推。持续这个过程，直到最小堆为空，表示已经完成合并。

算法总共需要 $n$ 步将 $n$ 个元素插入到最小优先级队列中，并且每次插入需要 $\lg k$ 时间，因此算法的时间复杂度是 $O(n \lg k)$。
