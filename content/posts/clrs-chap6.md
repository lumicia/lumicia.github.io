---
title: "算法导论 Chap6：堆、堆排序和优先级队列"
date: 2021-04-16T09:53:52+08:00
categories: ["Algorithm"]
tags: ["CLRS"]
draft: false
---

堆是一个数组，可用近似完全二叉树[^1]表示。表示堆的数组的两个属性：

- 数组长度；
- 堆大小。

堆大小表示数组中实际存储在堆中的数据。堆大小不小于 $0$，不大于数组长度。

```python
class MaxHeap(list):
    def __init__(self, data: list[int]) -> None:
        super().__init__(data)
        self.length = len(self)
        self.heap_size = self.length
        self.build_max_heap()
```

给定一个结点下标 $i$，则它的父结点下标 $\lfloor i / 2\rfloor$，左子结点下标 $2i$，右子结点下标 $2i + 1$。我们可以用三个简单的函数来计算父结点、左右子结点的下标。

```python
def parent(i: int) -> int:
    return (i - 1) // 2

def left(i: int) -> int:
    return 2 * i + 1

def right(i: int) -> int:
    return 2 * i + 2
```

二叉堆分为最大堆和最小堆，二者分别具有的性质：

- 最大堆中根结点最大；除了根结点以外，其他结点小于等于父结点。
- 最小堆中根结点最小；除了根结点以外，其他结点大于等于父结点。

堆中结点的**高度**（height）定义为该结点到叶结点最长简单路径上边的数量。堆的高度就是根结点的高度。

堆中结点的**深度**（depth）定义为该结点到根结点最长简单路径上边的数量。堆的深度就是叶结点的深度。

堆中结点的**度**（degree）定义为该结点的子结点数量。

堆的其他一些性质：

1. 含 $n$ 个元素的堆的高度为 $h = \lfloor \lg n \rfloor = \Theta(\lg n)$。
2. 含 $n$ 个元素的堆的叶结点下标分别为 $\lfloor n / 2 \rfloor + 1, \lfloor n / 2 \rfloor + 2, \cdots, n$。
3. 含 $n$ 个元素的堆至多有 $\left\lceil \cfrac{n}{2^{h + 1}} \right\rceil$ 个高度为 $h$ 的结点。

最大堆可以用来实现堆排序算法。最小堆通常用于构建优先级队列。

## 堆排序

堆排序通过最大堆来实现。堆排序算法可以分为三个过程：

- `max_hepify`：维持最大堆的性质。
- `build_max_heap`：从无序数组中构建最大堆。
- `heap_sort`：堆排序过程。

### 维持最大堆的性质

假设一个结点的左右子树（根结点分别为它的左右子结点）都是最大堆，但该结点可能比左右子结点小，此时就可以调用 `max_heapify` 将该结点在最大堆中「逐级下降」，直到以该结点为根结点的子树重新满足最大堆的性质为止。

```python
def max_heapify(self, i: int) -> None:
    l = self.left(i)
    r = self.right(i)

    if l <= self.heap_size - 1 and self[l] > self[i]:
        largest = l
    else:
        largest = i
    
    if r <= self.heap_size - 1 and self[r] > self[largest]:
        largest = r
        
    if largest != i:
        self[i], self[largest] = self[largest], self[i]
        self.max_heapify(largest)
```

`largest` 表示某个结点 `a[i]` 和它的左右子结点中最大的结点的下标。根据大小关系可以分为两种情况：

- 如果 `a[i]` 就是最大的结点，那么堆已经是最大堆，结束程序。
- 如果 `a[i]` 的某个子结点是最大的结点，那么需要将它和这个子结点交换位置（最大堆的根结点最大）。交换位置以后，`a[i]` 元素的位置的下标是 `largest`。它在这个位置也可能不满足最大堆性质，因此需要在以它为根结点的子树上递归地调用 `max_heapity`。

#### max_heapify 的复杂度

在以 `a[i]` 为根结点，大小为 $n$ 的子树上调用 `max_heapify` 的运行时间包括：

- 调整 `a[i]` 和它的左右子结点需要 $O(1)$ 时间；
- 在以 `a[i]` 的子结点为根的子树上递归调用 `max_heapify` 的时间。最坏情况发生在该子结点的子树有最多结点的时候。此时 `a[i]` 子树的底层是半满的，左子结点的子树叶结点深度都比右子结点的子树大 1。

现在需要计算最坏情况下左子结点的子树结点数量。对于高度为 $h$ 的堆，元素个数最多为 $2^{h + 1} - 1$（也就是一棵完全二叉树），这个堆中高度为 $h$ 的结点个数为 $2^h$。设右子结点的子树结点数量为 $k$，则左边与之对称的部分也有 $k$ 个结点，左边剩下的最底层的结点个数 $x = \cfrac{2^h}{2} = 2^{h - 1}$，而 $k = 2^h - 1 - x$，得 $k = 2^{h - 1} - 1$，因此 $x = k + 1$。 左子结点的子树结点个数为 $k + x = 2k + 1$。所有结点个数等于 `a[i]` 与左右子结点的子树结点数量之和，$n = 1 + (2k + 1) + k = 3k +2$，$k = \cfrac{n - 2}{3}$。则 $2k + 1 = 2 \cdot \cfrac{n - 2}{3} + 1 = \cfrac{2}{3} n - \cfrac{1}{3}$。

我们现在知道了最坏情况下左子结点的子树结点数量，可以得到递归式 $T(n) \leqslant T(2n / 3) + \Theta(1)$。根据主定理得到解为 $T(n) = O(\lg n)$。对于在树中高度为 $h$ 的结点来说，`max_heapify` 的时间复杂度是 $O(h)$。

### 构建最大堆

利用 `max_heapify` 从一个数组自底向上地构建最大堆。因为每个叶结点都可以看作是只含一个元素的堆，它们的构建是平凡的，我们可以从叶结点的前一个结点开始遍历。从性质 2 可得叶结点的前一个结点的下标是 $\lfloor n / 2 \rfloor$，因此循环从数组下标 $\lfloor n / 2 \rfloor$ 逆向开始直到数组首元素，对区间中的每个结点调用 `max_heapify`：

```python
def build_max_heap(self) -> None:
    self.heap_size = self.length
    for i in range(self.length // 2 - 1, -1, -1):
        self.max_heapify(i)
```

不同结点运行 `max_heapify` 的时间与它的高度有关，且高度都较小。利用性质 1 和 3，与级数微分公式 $A.8$ 可以得到 `build_max_heap` 的运行时间为：

$$
\sum_{h = 0}^{\lfloor \lg n \rfloor} \left\lceil\frac{n}{2^{h + 1}} \right\rceil O(h) = O\left(n \sum_{h = 0}^{\lfloor \lg n \rfloor} \frac{h}{2^h}\right) = O\left(n \sum_{h = 0}^{\infty} \frac{h}{2^h}\right) = O(n)
$$

因此 `build_max_heap` 函数的时间复杂度是 $O(n)$。

### 堆排序过程

利用 `build_max_heap` 先将数组构建为最大堆，数组最大元素必然为 `a[1]`，然后将 `a[1]` 和 `a[n]` 交换。接着对前 $n - 1$ 个元素的子数组构建新的最大堆，交换首尾元素，重复这个过程直到堆的大小为 $2$。

```python
def heap_sort(self) -> None:
    self.build_max_heap()
    for i in range(self.length - 1, 0, -1):
        self[0], self[i] = self[i], self[0]
        self.heap_size -= 1
        self.max_heapify(0)
```

`heap_sort` 调用一次 `build_max_heap`，调用 $n - 1$ 次 `max_heapify`，因此 `heap_sort` 的时间复杂度是 $O(n \lg n)$。

## 优先级队列

优先级队列是一个集合，其中的每一个元素都有与之关联的值，称为键。优先级队列可以通过键来记录元素之间的相对优先级。优先级队列分为最大优先级队列和最小优先级队列。

最大优先级队列支持的操作：

- `max_heap_insert`：把元素插入到队列中。
- `heap_maximum`：返回队列中键最大的元素。
- `heap_extract_max`：从队列中移除并返回键最大的元素。
- `heap_increase_key`：将元素的键增加到给定值，该值应当大于之前的值。

`heap_maximum` 过程：

```python
def heap_maximum(self) -> int:
    return self[0]
```

最大堆的首元素是最大的，直接返回即可。

`heap_extract_max` 过程：

```python
def heap_extract_max(self) -> int:
    if self.heap_size < 1:
        sys.exit('heap underflow')

    maximum = self[0]
    self[0] = self[self.heap_size - 1]
    self.heap_size -= 1
    self.max_heapify(0)

    return maximum
```

对大小小于 $1$ 的堆使用 `heap_extract_max` 应当报错。用 `maximum` 变量保存堆的最大值，然后将堆中最后一个元素放到堆的开头，并把堆的大小减一。调用 `max_heapify` 使堆保持最大堆性质。最后返回 `maximum`。

`heap_increase_key` 过程：

```python
def heap_increase_key(self, i: int, key: int | float) -> None:
    if key < self[i]:
        sys.exit('new key is smaller than current key')

    self[i] = key
    while i > 0 and self[self.parent(i)] < self[i]:
        self[self.parent(i)], self[i] = self[i], self[self.parent(i)]
        i = self.parent(i)
```

如果新的键小于原来的键，应当报错。将键更新后，为了维持最大堆性质，在当前结点到根结点的路径上寻找合适的位置插入这个新的键。每次比较当前元素和父结点的大小，如果当前元素的键较大，则与父结点的键交换。重复比较的过程直到当前元素的键小于父结点为止，此时堆符合最大堆性质。

`max_heap_insert` 过程：

```python
def max_heap_insert(self, key: int) -> None:
    if self.heap_size >= self.length:
        sys.exit('heap overflow')

    self.heap_size += 1
    self[self.heap_size - 1] = float('-inf')
    self.heap_increase_key(self.heap_size - 1, key)
```

要插入元素时，将堆的大小加 $1$，先把元素放在堆的最后一个位置，键设为负无穷。然后调用 `heap_increase_key` 把元素的键的值增加到给定的值，这样就保持了堆的最大堆性质。

优先级队列的所有操作都可以在 $O(\lg n)$ 时间内完成。

## 本章难点

1. 自底向上建堆的时间复杂度为 $O(n)$。
2. 优先级队列的 `heap_increase_key` 过程能够保持最大堆性质。
---

[^1]: CLRS 中定义的几种二叉树的名称和其他教材有差异，需要注意一下。CLRS 中常用的二叉树有：完全二叉树、近似完全二叉树和满二叉树。
