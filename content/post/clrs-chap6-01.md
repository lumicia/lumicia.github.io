---
title: "算法导论 Chap6 01：堆、堆排序和优先级队列"
date: 2021-08-11T09:53:52+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: false
---

堆是一个数组，可用完全二叉树表示。

表示堆的数组 `A` 的两个属性：

- 数组长度 `A.length`；
- 堆大小 `A.heap_size`。

堆大小不小于 0，不大于数组长度。

给定一个结点下标 $i$，则它的父结点下标 $\lfloor i / 2\rfloor$，左子结点下标 $2i$，右子结点下标 $2i + 1$​。

`PARENT(i)`

```
return floor(i / 2)
```

`LEFT(i)`

```
return 2i
```

`RIGHT(i)`

```
return 2i + 1
```

二叉堆分为最大堆和最小堆，二者分别具有的性质：

- 最大堆中根结点最大，除了根结点以外，其他结点小于等于父结点。
- 最小堆中根结点最小，除了根结点以外，其他结点大于等于父结点。

堆中结点的高度定义为该结点到叶结点最长简单路径上边的数量，堆的高度从而就是根结点的高度。$n$ 个结点的堆高度 $h = \lfloor \lg n \rfloor = \Theta(n)$。

最大堆可以用来实现堆排序算法。最小堆可以用于构建优先级队列。

## 堆排序

堆排序通过最大堆来实现。

堆排序算法可以分为三个过程：

- `MAX-HEAPIFY`：维持最大堆的性质。
- `BUILD-MAX-HEAP`：从无序数组中构建最大堆。
- `HEAPSORT`：堆排序过程。

### 维持最大堆的性质

一个结点的左右子树都是最大堆，但它比子结点小，此时就可以调用 `MAX-HEAPIFY` 将该结点在最大堆中「下降」，直到其满足最大堆的性质为止。

`MAX-HEAPITY(A, i)`：

```
l = LEFT(i)
r = RIGHT(i)
if l <= A.heap_size and A[l] > A[i]
	largest = l
else largest = i
if r <= A.heap_size and A[r] > A[largest]
	largest = r
if largest != i
	exchange A[i] with A[largest]
	MAX-HEAPIFY(A, largest)
```

### 构建最大堆

利用 `MAX-HEAPIFY` 过程自底向上来构建最大堆。

`BUILD-MAX-HEAP(A)`：

```
A.heap_size = A.length
for i = floor(A.length / 2) downto 1
	MAX-HEAPIFY(A, i)
```

### 堆排序过程

利用 `BUILD-MAX-HEAP` 从数组构建最大堆。

`HEAP-SORT(A)`：

```
BUILD-MAX-HEAP(A)
for i = A.length downto 2
	exchange A[1] with A[i]
	A.heap_size = A.heap_size - 1
	MAX-HEAPIFY(A)
```

## 优先级队列

优先级队列是一个集合，其中的每一个元素都有与之关联的值，称为键。优先级队列可以通过键来记录元素之间的相对优先级。优先级队列分为最大优先级队列和最小优先级队列。

最大优先级队列支持的操作：

- `INSERT(S, x)`
- `MAXIMUM(S)`
- `EXTRACT-MAX(S)`
- `INCRESE-KEY(S, x, k)`

`HEAP-MAXIMUM(A)` 过程实现 `MAXIMUM` 操作：

```
return A[1]
```

`HEAP-EXTRACT-MAX(A)` 过程实现 `EXTRACT-MAX` 操作：

```
if A.heap_size < 1
	error "heap underflow"
max = A[1]
A[1] = A[A.heap_size]
MAX-HEAPIFY(A, 1)
return max
```

`HEAP-INCREASE-KEY(A, i, key)` 过程实现 `INCREASE-KEY` 操作：

```
if key <A[i]
	error "new key is smaller than current key"
A[i] = key
while i > 1 and A[PARENT(i)] < A[i]
	exchange A[i] with A[PARENT(i)]
	i = PARENT(i)
```

`MAX-HEAP-INSERT(A, key)` 过程实现 `INSERT` 操作：

```
A.heap_size = A.heap_size + 1
A[A.heap_size] = -Infinity
HEAP-INCREASE-KEY(A, A.heap_size, key)
```

优先级队列的所有操作都可以在 $O(\lg n)$ 时间内完成。
