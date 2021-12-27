---
title: "算法导论 Chap6 习题"
date: 2021-04-22T13:10:05+08:00
categories: ["Algorithm"]
tags: ["CLRS"]
draft: false
---

## 6.1

### 6.1-1

每层都完全填满的二叉堆第 $i$ 层元素个数是 $2^{i - 1}$。

由几何级数公式 $A.5$，高度为 $h$ 的堆中元素个数最多为 $\displaystyle \sum_{i = 1}^{h + 1} 2^{i - 1} = \cfrac{2^{h + 1} - 1}{2 - 1} = 2^{h + 1} - 1$，最少为 $1 + \displaystyle \sum_{i = 1}^h 2^{i - 1} = 1 + \cfrac{2^h - 1}{2 - 1} = 2^h$。

### 6.1-2

设该堆的高度为 $h$，由 6.1-1 有 $2^h \leqslant n \leqslant 2^{h + 1} - 1$。

两边取对数得 $\lg(n + 1) - 1 \leqslant h \leqslant \lg n$。

故高度为 $\lfloor \lg n \rfloor$。

### 6.1-3

假设该子树中存在一个非根结点的最大元素，则它一定大于它的父结点，这违背了最大堆性质，因此这样的结点不存在。故子树中最大元素一定位于根结点。

### 6.1-4

假如最小元素所在结点有子结点，则违背最大堆性质，因此最小元素一定位于叶结点。

### 6.1-5

对于一个已排序的数组 $A$ 中的元素 $A[i]$，一定有 $A[\text{PARENT}(i)] \leqslant A[i]$，其中 $i > 1$，满足最小堆性质，所以 $A$ 是最小堆。

### 6.1-6

不是。$A[9] = 7$ 的父结点 $A[\text{PARENT}(9)] = 6$，不满足最大堆性质。

### 6.1-7

对于下标为 $\lfloor n / 2 \rfloor + 1$ 的结点，假设它有左子结点，则左子结点下标为：
<div>
$$
\begin{aligned}
\text{LEFT}(\lfloor n / 2 \rfloor + 1) & = 2(\lfloor n / 2 \rfloor + 1) \\
& > 2(n / 2 + 1) - 2 \\
& = n + 2 - 2 \\
& = n
\end{aligned}
$$
</div>
因为下标为 $n$ 的结点一定是堆的最后一个结点，而它的左子结点下标大于 $n$，所以不存在。故下标为 $\lfloor n / 2 \rfloor + 1$ 的结点是叶结点。从而下标大于 $\lfloor n / 2 \rfloor + 1$ 的结点也都是叶结点。

接着证明它是第一个叶结点。考察它的前一个结点，即下标为 $\lfloor n / 2 \rfloor$ 的结点。同理可推出当 $n$ 为偶数时，该结点的左子结点下标是 $n$；$n$ 为奇数时，该结点的右子结点下标是 $n$，也就是说该结点是堆中最后一个叶结点的父结点，因此该结点不是叶结点。从而下标 $\lfloor n / 2 \rfloor + 1$ 是堆的第一个叶结点。

堆的叶结点总数为 $n - (\lfloor n / 2 \rfloor + 1) + 1 = \lceil n / 2 \rceil$。

## 6.2

### 6.2-1

略。

### 6.2-2

`MIN-HEAPIFY(A, i)`：

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

### 6.2-3

没有影响，函数直接返回。

### 6.2-4

此时，该结点是叶结点，因此没有影响，函数直接返回。

### 6.2-5

`MAX-HEAPIFY-ITERATIVE(A, i)`：

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

### 6.2-6

最坏情况是从根结点一直递归到叶结点，因为堆的高度为 $\lfloor \lg n \rfloor$，所以最坏情况运行时间为 $\Omega(\lg n)$。

## 6.3

### 6.3-1

略。

### 6.3-2

如果从下标 `1` 开始，子树不一定能维持最大堆性质，`MAX-HEAPIFY` 调用可能失败。

### 6.3-3

用数学归纳法来证明。

基本情况：证明高度 $h=0$ 时命题为真。

当 $h = 0$ 时，等价于证明最多有 $\lceil \cfrac{n}{2^{h + 1}} \rceil$ 个叶结点。由 6.1-7 知道所有叶结点下标，可算出叶结点总数为 $n\ - (\lfloor \cfrac{n}{2} \rfloor + 1) + 1 = n - \lfloor \cfrac{n}{2} \rfloor = \lceil \cfrac{n}{2} \rceil$。

归纳情况：当高度为 $h-1$ 时命题成立，证明对高度为 $h$ 时仍然成立。

设树 $T$ 在高度为 $h$ 的结点个数 $n_h$，叶结点个数 $n_0$。$T$ 删除所有叶结点得到的子树 $T'$。

$T'$ 的所有结点个数 $n' = n - n_0$。从基本情况有 $n_0 = \lceil \cfrac{n}{2} \rceil$，因此 $n' = n - \lceil \cfrac{n}{2} \rceil = \lfloor \cfrac{n}{2} \rfloor$。

$T$ 中高度为 $h$ 的结点在 $T'$ 中高度为 $h-1$，设其数量为 $n'_{h - 1}$。则 $n_h = n'_{h - 1}$。

因为对 $n'_{h - 1}$ 命题 $n'_{h - 1} \leqslant \lceil \cfrac{n'}{2^h} \rceil$ 成立，所以：
<div>
$$
\begin{aligned}
n_h
& = n'_{h - 1} \\
& \leqslant \left \lceil \frac{n'}{2^h} \right \rceil \\
& = \left \lceil \frac{\lfloor \frac{n}{2} \rfloor}{2^h} \right \rceil \\
& \leqslant \left \lceil \frac{\frac{n}{2}}{2^h} \right \rceil \\
& = \left \lceil \frac{n}{2^{h + 1}} \right \rceil
\end{aligned}
$$
</div>
所以命题对 $h$ 也成立，故原命题成立。

## 6.4

### 6.4-1

略。

### 6.4-2

初始化：子数组 $A[i+1..n]$ 为空，因此不变式正确。

保持：$A[1]$ 在 $A[1..i]$ 中最大，并且小于 $A[i+1..n]$ 中的元素。当把 $A[1]$ 放到第 $i$ 个位置，则 $A[i..n]$ 已经是排好序的。递减 `heap_size` 并调用 `MAX-HEAPIFY` 会将 $A[1..i-1]$ 变成最大堆。递减 $i$ 的值为下次迭代建立不变式。

终止：在循环结束时，$i = 1$。此时 $A[2..n]$ 是排好序的并且 $A[1]$ 是数组中最小的元素，这让整个数组 $A[1..n]$ 也是排好序的。

### 6.4-3

两种情况都是 $\Theta(n \lg n)$。

如果数组以升序排序，将数组转化成堆需要 $O(n)$ 时间。然后需要 $ n- 1$ 次调用过程 `MAX-HEAPIFY`，每次都执行 $\lg k$ 次操作。因此
$$
\sum_{k = 1}^{n - 1} \lg k = \lg((n - 1)!) = \Theta(n \lg n)
$$
降序同理。`BUILD-MAX-HEAP` 更快（常数因子），但整个计算时间由循环中的 `HEAPSORT` 主导，因此：
$$
\sum^n_{i = 1} \lg i = \lg n! = \Theta(n \lg n)
$$

### 6.4-4

当数组已经排好序时，需要线性时间将它转化成最大堆，然后需要 $n \lg n$ 时间来排序。

### 6.4-5 $\star$

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

## 6.5

### 6.5-1

略。

### 6.5-2

略。

### 6.5-3

`HEAP-MINIMUM(A)`：

```
return A[1]
```

`HEAP-EXTRACT-MIN(A)`：

```
if A.heap_size < 1
	error "heap underflow"
min = A[1]
A[1] = A[A.heap_size]
A.heap_size = A.heap_size - 1
MIN-HEAPIFY(A, 1)
return min
```

`HEAP-DECREASE-KEY(A, i, key)`：

```
if key > A[i]
	error "new key is larger than current key"
A[i] = key
while i > 1 and A[PARENT(i)] > A[i]
	exchange A[i] with A[PARENT]
	i = PARENT(i)
```

`MIN-HEAP-INSER(A, key)`：

```
A.heap_size = A.heap_size + 1
A[A.heap_size] = Infinity
HEAP-DECRASE-KEY(A, A.heap_size, key)
```

### 6.5-4

为了维持 `HEAP-INCREASE-KEY` 过程的 `if` 语句的条件为真。

### 6.5-5

初始化：`A[1..heap-size]` 满足最大堆性质，除非 $A[i]$ 大于 $A[PARENT(i)]$，因为 $A[i]$ 被修改了。$A[i]$ 大于它的子结点，否则 `if` 语句会失败，不会进入 `while` 循环。

保持：在循环中交换 $A[i]$ 和 $A[PARENT(i)]$，满足了最大堆性质。然后在循环下次迭代继续和父结点的父结点比较，继续进行交换，以继续维持最大堆性质。

终止：当堆中元素遍历完，或 $A[i]$ 小于父结点时（维持了最大堆性质），结束循环。此时，$A$ 是最大堆。

### 6.5-6

`HEAP-INCREASE-KEY(A, i, key)`：

```
if key < A[i]
	error "new key is smaller than current key"
while i > 1 and A[PARENT(I)] < key
	A[i] = A[PARENT(i)]
	i = PARENT(i)
A[i] = key
```

### 6.5-7

对于队列，优先级队列每次插入新元素时递减所添加元素的优先级。

对于栈，优先级队列则以递增优先级的方式添加元素。

### 6.5-8

`HEAP-DELETE(A, i)`：

```
if A[i] > A[A.heap-size]
	A[i] = A[A.heap-size]
	MAX-HEAPIFY(A, i)
else HEAP-INCREASE-KEY(A, i, A[A.heap-size])
A.heap-size = A.heap-size - 1
```

### 6.5-9

从 $k$ 个有序链表中分别从头取一个元素，然后放到一个最小堆实现的最小优先级队列中。

合并时，从最小堆中取出最小的元素，插入该元素对应链表中的下一个元素到最小堆中。如果链表为空，则从最小堆中取出倒数第二小的元素，以此类推。持续这个过程，直到最小堆为空，表示已经完成合并。

算法总共需要 $n$ 步将 $n$ 个元素插入到最小优先级队列中，并且每次插入需要 $\lg k$ 时间，因此算法的时间复杂度是 $O(n \lg k)$。

## 思考题

### 6-1

a. 不是相同的。最大堆对于结点的左右子结点的顺序没有要求。对于数组 `[1, 2, 3, 4, 5]`，自底向上地遍历数组元素构建最大堆得到 `[5, 4, 3, 1, 2]`，而自顶向下地插入数组元素构建最大堆得到 `[5, 4, 2, 1 ,3]`。

b. 每次插入最多需要 $O(\lg n)$ 时间，总共有 $n - 1$ 个元素要插入，时间复杂度为 $(n - 1) \cdot O(\lg n) = O(n \lg n)$。因此插入法在效率方面不如自底向上的 Floyd 建堆算法。

### 6-2

a. 效仿二叉堆，给定 $d$ 叉堆中一个结点的下标 $i$，计算出它的父结点和子结点的下标。堆的第一个结点下标是 $1$，它的子结点下标是 $1 + 1,1 + 2,\cdots,1 + d = 2,3,\cdots,1 + d$。第二个结点的第一个子结点下标是 $(1 + d) + 1$，第二个是 $(1 + d) + 2$，$\cdots$，第 $d$ 个是 $(1 + d) + d$。第三个结点的第一个子结点下标是 $(1 + 2d) + 1$，第二个是 $(1 + 2d) + 2$，$\cdots$，第 $d$ 个是 $(1 + 2d) + d$。以此类推，可以归纳出下标为 $i$ 的结点的第 $j$ 个子结点下标是 $1 + (i - 1)d +j$。反过来可以推出父结点下标是 $\cfrac{i - 1 - j}{d} + 1$，此时可以设 $j = 1$，则父结点下标为 $\lfloor \cfrac{i - 2}{d} + 1 \rfloor$。

`D-ARY-PARENT(i)`：

```
return floor((i - 2) / d + 1)
```

`D-ARY-CHILD(i, j)`：

```
return (i - 1)d + j + 1
```

b. 高度是 $\Theta(\log_d n)$。

c. `D-ARY-EXTRACT-MAX(A)`：

```
if A.heap_size < 1
    error "heap underflow"
max = A[1]
A[1] = A[A.heap_size]
A.heap_size = A.heap_size - 1
D-ARY-MAX-HEAPIFY(A, 1)
return max
```

`D-ARY-MAX-HEAPIFY(A, i)`：

```
largest = i
for j = 1 to d
    if D-ARY-CHILD(j, i) <= A.heap_size and A[D-ARY-CHILD(j, i)] > A[i]
        if A[D-ARY-CHILD(j, i)] > largest
            largest = A[D-ARY-CHILD(j, i)]
if largest != i
    exchange A[i] with A[largest]
    D-ARY-MAX-HEAPIFY(A, largest)
```

`D-ARY-MAX-HEAPIFY` 的时间复杂度和结点的高度有关，等于 $O(d \log_{d}n)$。`D-ARY-EXTRACT-MAX` 除了调用 `D-ARY-MAX-HEAPIFY` 以外的操作都是常数项的，因此时间复杂度是 $O(d \log_{d}n)$。

d. `D-ARY-HEAP-INSERT(A, key)`：

```
A.heap_size = A.heap_size + 1
A[A.heap_size] = key
i = A.heap_size
whild i > 1 and A[D-ARY-PARENT] < A[i]
    exchange A[i] with A[D-ARY-PARENT]
    i = D-ARY-PARENT(i)
```

时间复杂度是 $O(\log_{d}n)$，因为 `while` 循环运行次数最多和堆高相同。

e. `D-ARY-HEAP-INCREASE-KEY(A, i, key)`：

```
if key < A[i]
    error "new key is smaller than current key"
A[i] = key
while i > 1 and A[D-ARY-PARENT] < A[i]
    exchange A[i] with A[D-ARY-PARENT]
    i = D-ARY-PARENT(i)
```

时间复杂度是 $O(\log_{d}n)$，因为 `while` 循环运行次数最多和堆高相同。

### 6-3

TODO