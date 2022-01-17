---
title: "算法导论 Chap7：快速排序"
date: 2021-04-18T09:42:04+08:00
categories: ["Algorithm"]
tags: ["CLRS", "Python"]
draft: true
---

快速排序同样采用分治的策略对数组进行排序。快速排序的基本思想是选取一个 pivot 进行原地排序，使得数组的一部分小于等于 pivot，另一部分大于等于 pivot，然后把 pivot 交换到两部分的中间，将数组分为左右两个子数组。接着对两个子数组进行相同的操作，直到子数组只包含一个元素，此时整个数组完成了排序。

`quick_sort`：

```python
def quick_sort(a: list[int], p: int, r: int) -> None:
	if p < r:
		q = partition(a, p, r)
		quick_sort(a, p, q - 1)
		quick_sort(a, q + 1, r)
```

`partition`：

```python
def partition(a: list[int], p: int, r: int) -> int:
    x = a[r]
    i = p - 1
    for j in range(p, r):
        if a[j] <= x:
            i += 1
            a[i], a[j] = a[j], a[i]

    a[i + 1], a[r] = a[r], a[i + 1]
    return i + 1
```

## 快速排序的性能

快速排序的性能与所选取的 pivot 有很大关系。pivot 将数组划分为两部分，如果两部分的元素数量接近，则是平衡的，否则是不平衡的。

对于包含 $n$ 个元素的数组，最平衡的划分使得快速排序性能接近归并排序 $O(n \lg n)$，两个部分包含元素个数小于等于 $\cfrac{n}{2}$。

最不平衡的划分使性能接近插入排序 $O(n^2)$，两个部分分别包含 $n - 1$ 个和 $0$ 个元素。

平均情况下的快速排序性能仍然是 $O(n \lg n)$，只不过常数因子略大一些。

为了得到期望的性能，可以通过随机选取 pivot 的方法得到随机化的快速排序，在数组元素互异的情况下，快速排序的期望性能是 $O(n \lg n)$。

## 随机化的快速排序

`randomized_quick_sort`：

```python
def randomized_quick_sort(a: list[int], p: int, r: int) -> None:
    if p < r:
        q = randomized_partition(a, p, r)
        randomized_quick_sort(a, p, q - 1)
        randomized_quick_sort(a, q + 1, r)
```

`randomized_partition`：

```python
def randomized_partition(a: list[int], p: int, r: int) -> int:
    i = random.randint(p, r)
    a[i], a[r] = a[r], a[i]
    return partition(a, p, r)
```
