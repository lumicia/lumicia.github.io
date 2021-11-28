---
title: "算法导论 Chap2 习题"
date: 2021-04-02T14:52:05+08:00
categories: ["Algorithm"]
tags: ["CLRS"]
draft: false
---

## 2.1

### 2.1-1

略。

### 2.1-2

```
for j = 2 to A.length
	key = A[j]
	i = j - 1
	while i > 0 and A[i] < key
		A[i + 1] = A[i]
		i = i - 1
	A[i + 1] = key
```

修改成 ` A[i] < key` 即可。

<!--more-->

### 2.1-3

```
for i = 1 to A.length
	if A[i] == v
		return i
return NIL
```

循环不变式：每次 `for` 循环开始迭代时，子数组 $A[1..i-1]$ 由均不等于 $v$ 的元素组成。

初始化：证明循环不变式在循环开始前为真。循环开始前 $i=1$。子数组为空，因此为真。

保持：证明循环不变式在循环的下一次迭代时仍为真。将 $A[i]$ 和 $v$ 相比较，如果二者相同，就返回 $i$；否则循环继续下一次迭代。每次循环的迭代结束时，子数组 $A[1..i]$ 中不包含 $v$，因此循环不变式仍为真。在循环下一次迭代中递增 $i$ 的值将维持循环不变式。

终止：证明循环不变式在循环终止时仍为真。让循环结束的条件为 $i>A.length=n$，确切地说此时必定有 $i=n+1$。将 $i$ 替换为 $n+1$，则子数组 $A[1..n]$ 由均不等于 $v$ 的元素组成，因此返回 $\text{NIL}$。观察到子数组 $A[1..n]$ 等于原来的整个数组，可以得出结论原数组中不包含等于 $v$ 的元素。故算法正确。

### 2.1-4

因为涉及到进位，需要考虑如何计算相加后 $C$ 中的数字。使用一个变量来保存 $A$ 和 $B$ 每个位相加时的进位。使用模运算可以得到相加后的余数，使用除法求进位的数字。

举个例子： $A = [1, 0, 0, 1]$，$B = [1, 1, 0, 1]$，则 $C = [1, 0, 1, 1, 0]$。

形式化描述：

输入: $A$ 和 $B$ 都是二进制数字的数组，长度均为 $n$​。

$\forall i\in\{1\dots n\}:\ A[i],B[i]\in\{0,1\}$.

输出: 二进制数字的数组 $C$，长度为 $n+1$。

$\forall i\in\{1\dots n+1\}:\ C[i]\in\{0,1\}$:
$$
\sum_{i=1}^{n+1}C[i]*2^{i-1}=\sum_{j=1}^{n}A[j]*2^{j-1}+\sum_{k=1}^{n}B[k]*2^{k-1}
$$


```
let C[A.length + 1] be new array
carry = 0
for i = A.length downto 1
	// remainder
	C[i + 1] = (A[i] + B[i] + carry) % 2
	// quotient
	carry = (A[i] + B[i] + carry) / 2
C[1] = carry
return C
```

## 2.2

### 2.2-1

$\Theta(n^3)$。

### 2.2-2

```
for i = 1 to A.length - 1
	min = A[i]
	for j = i + 1 to A.length
		if A[i] > A[j]
			min = A[j]
	swap(A[i], min)
```

1、循环不变式：在 `for` 循环的开始，子数组 $A[1..i-1]$ 由数组 $A$ 中已排序的最小的 $i-1$ 个元素组成。

初始化：循环开始前 $i=1$，子数组为空，因此为真。

保持：循环每次迭代结束时，将最小值放到 $A[i]$ 的位置，子数组 $A[1..i-1]$ 中的元素都比 $A[i]$ 小，且已排序，因此循环不变式为真。在循环下一次迭代中递增 $i$ 的值将维持循环不变式。

终止：循环结束的条件为 $i=A.length=n$。此时子数组 $A[1..n-1]$ 由原来数组 $A[1..n]$ 的 $n-1$ 个元素组成，且已排序。而剩下的元素则保证不小于子数组中的元素，因此整个数组都是有序的。故算法正确。

2、前 $n-1$ 个元素排序完成后，子数组 $A[1..n-1]$ 由数组 $A$ 中已排序的最小的 $n-1$ 个元素组成，因此第 $n$ 个元素必定是数组中最大的元素，此时它已经在正确的位置，无需再排序。

3、选择排序最好的情况（数组已排序）和最坏的情况（数组逆序）都需要将外循环和内循环的每一条语句执行一遍，所以都需要 $\Theta(n^2)$ 时间。

### 2.2-3

1、分析在数组 $A[a_1,a_2,\dots,a_n]$ 中进行线性查找的平均情况：第 1 个元素需要查找 1 个元素，第 2 个元素查找 2 个，以此类推，第 n 个元素查找 n 个。总共查找 n 次，因此平均个数为 $\cfrac{1+2+\dots+n}n=\cfrac{n+1}2$ 个。

2、最坏情况是查找到第 $n$ 个元素。

3、平均情况和最坏情况的运行时间都是 $\Theta(n)$。

### 2.2-4

没看懂问题，看了下官方的答案，应该是通过添加一个特殊的情况，如果输入符合这个特例，则输出预先计算的结果，这样就可以得到最好情况的运行时间了。

## 2.3

### 2.3-1

略。

### 2.3-2

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
    A[k..r] = L[i..n1]
```

第 19 行只需要判断 `j` 是否等于右子数组的长度，因为左子数组的长度是大于等于右子数组的。如果右子数组已遍历完，则表示左子数组中剩余的元素都大于循环开始时右子数组的任意元素，并且已排序。此时只需要将左子数组中的剩余元素复制到合并数组中即可。

### 2.3-3

以下用 $\lg n$ 表示 $\log_2 n$。

基本情况：当 $n=2$ 时，$T(n)=2 \lg 2 = 2$。

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

### 2.3-4

最坏情况下将 $A[n]$ 插入到已排序数组 $A[1..n - 1]$ 需要的时间为 $\Theta(n)$，因此
$$
T(n) =
\begin{cases}
\Theta(1) &\ \text{if}\ n = 1, \\\\
T(n - 1) + \Theta(n) &\ \text{if}\ n>1.
\end{cases}
$$
递推式的结果是 $\Theta(n^2)$。

递归式插入排序的关键操作是「合并」步骤中将 $A[n]$ 插入到已排序数组 $A[1..n - 1]$ 中。这一过程用辅助函数 `insert(A, n)` 表示。

只要向排序主函数 `insertion_sort_recursive(A, n)` 传递的数组 $A$ 长度大于 0 ，就可以递归调用它自身，然后调用 `insert(A, n)` 合并。

### 2.3-5

迭代式：`ITERATIVE-BINARY-SEARCH(A, v, lo, hi)`

```
while lo <= hi
    mid = floor((lo + hi) / 2)
    if A[mid] == v
        return mid
    else if A[mid] 
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
if A[mid] == v
    return mid
else if A[mid] < v
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
递归式的结果是 $\Theta(\lg n)$。

### 2.3-6

不能。查找虽然是 $\Theta(\lg n)$，但移动元素仍然是 $\Theta(n)$。

### 2.3-7 $\star$

首先对 $S$ 排序，需要 $\Theta(n\lg n)$ 时间。

然后对于 $S$ 中的元素 $e_i$，$i = 1,\dots,n$，用二分查找在 $A[i + 1..n]$ 中查找 $e_j = x - e_i$，需要 $\Theta(\lg n)$ 时间。

如果找到 $e_j$，返回 $j$ 的值；否则进行下一次迭代。

算法总共所需时间 $\Theta(n\lg n) + n\cdot \Theta(\lg n)=\Theta(n\lg n)$。

## 思考题

### 2-1

a. 对长度为 $k$ 的子表使用插入排序的最坏情况需要 $\Theta(k^2)$ 时间，因此对 $n / k$ 个长度为 $k$ 的子表使用插入排序的最坏情况需要 $n / k \cdot \Theta(k^2) = \Theta(nk)$ 时间。

b. 因为有 $n / k$ 个子表，每次将两个子表合并，需要合并的次数是 $\lg (n / k)$；每次合并最坏时间为 $\Theta(n)$，所以将所有子表合并的最坏情况是 $\Theta(n \lg (n / k))$。

c. 根据题意有 $\Theta (nk + n \lg (n / k)) \leqslant \Theta(n \lg n)$。因此 $nk$ 对整体时间的影响应该小于 $ n\lg n$，$k$ 的最大值为 $\lg n$。

d. 选择让插入排序比归并排序快时的 $k$ 的值。

### 2-2

a. $A'$ 中的元素都是 $A$ 中的元素，不过已排序。

b. 循环不变式：第 2-4 行的 `for` 循环每次迭代开始时，子数组 `A[j..n]` 由原来在 `A[j..n]` 中的元素组成，但顺序可能发生了变化，且首元素 `A[j]` 是子数组中最小的元素。

初始化：循环第一次迭代开始时，子数组仅包含单个元素 `A[n]`，因此是有序的。

保持 ：每次迭代都将 `A[j]` 和 `A[j - 1]` 比较，并让较小的元素交换到下标较小的位置。迭代完成后，子数组 `A[j..n]` 递增一个元素，且首元素是子数组中最小的元素。

终止：导致 `for` 循环终止的条件是 `j = i`。在循环不变式中用 `i` 代替 `j`，有：子数组 `A[i..n]` 由原来在 `A[i..n]` 中的元素组成，`A[i]` 是子数组中最小的元素。

c. 循环不变式：第 1-4 行的 `for` 循环每次迭代开始时，子数组 `A[1..i - 1]` 由数组 `A[1..n]` 中最小的 `i - 1` 个元素组成，且已排序。子数组 `A[i..n]` 由 `A[1..n]` 中剩余的元素组成。

初始化：循环第一次迭代开始时，子数组 `A[1..i - 1]` 是空数组。

保持：内循环每次迭代后，`A[i]` 都会成为子数组 `A[i..n]` 中最小的元素。在下次外循环开始前，`A[1..i - 1]` 由比 `A[i..n]` 中更小的元素组成，且已排序。外循环迭代后，`A[1..i]` 将包含比 `A[i + 1..n]` 更小的元素，且已排序。

终止：当 `i = A.length` 时，循环终止。此时 `A[1..n]` 将由所有已排序的元素组成。

d. 第 `i` 次外循环将执行 `n - i` 次内循环，所以最坏情况运行时间是 $\Theta(n^2)$。理论上运行时间和插入排序一样，但由于多次交换操作，实际会更慢。

### 2-3

a. $\Theta(n)$。

b. `NAIVE-POLYNOMIAL-EVALUATION`：

```
y = 0
for i = 0 to n
    m = 1
    for k = 1 to i
        m = m * x
    y = y + a[i] * m
```

运行时间是 $\Theta(n^2)$，比 Horner 规则慢许多。

c. 初始化：一开始没有项，$y = 0$。

保持：在第 $i$ 次迭代，有

$$ \begin{aligned} y & = a_i + x \sum_{k = 0}^{n - (i + 1)} a_{k + i + 1} x^k \\ & = a_i x^0 + \sum_{k = 0}^{n - i - 1} a_{k + i + 1} x^{k + 1} \\ & = a_i x^0 + \sum_{k = 1}^{n - i} a_{k + i} x^k \\ & = \sum_{k = 0}^{n - i} a_{k + i} x^k. \end{aligned} $$

终止：当 $i = -1$ 时终止，将 $i = 0$ 代入，得 $\displaystyle y = \sum_{k = 0}^{n - i - 1} a_{k + i + 1} x^k = \sum_{k = 0}^n a_k x^k$。

d. 在 c 中证明了循环不变式，循环不变式的和等于给定系数的多项式。

### 2-4

a. $(1, 5)$，$(2, 5)$，$(3, 4)$，$(3, 5)$，$(4, 5)$。

b. 集合 $\{1, 2, \ldots, n\}$ 的逆序构成的数组 $\langle n, n - 1, \cdots, 1\rangle$ 具有最多的逆序对。逆序对的个数为 $(n - 1) + (n - 2) + \cdots + 1 = \cfrac{n(n - 1)}{2}$。

c. 插入排序的运行时间正比于逆序对的数量。

d. 首先需要清楚逆序对的构成。如果对数组进行归并排序来计算逆序对，则数组的逆序对由子数组中的逆序对和跨子数组的逆序对两部分组成。实际只要计算跨子数组的逆序对，因为子数组中的逆序对可以通过递归计算它自己的两部分逆序对得到。

`CROSS-INVERSION-COUNT(A, p, q, r)`：

```
n1 = q - p + 1
n2 = r - q
let L[1..n1 + 1] and R[1..n2 + 1] be new arrays
    for i = 1 to n1
L[i] = A[p + i - 1]
    for j = 1 to n2
R[j] = A[q + j]
L[n1 + 1] = ∞
R[n2 + 1] = ∞
i = 1
j = 1
inversions = 0
for k = p to r
    if L[i] <= R[j]
        A[k] = L[i]
        i = i + 1
    else
        inversions = inversions + n1 - i + 1
        A[k] = R[j]
        j = j + 1
return inversions
```

`INVERSIONS-COUNT(A, p, r)`：

```
if p < r
    q = floor((p + r) / 2)
    left = INVERSION-COUNT(A, p, q)
    right = INVERSION-COUNT(A, q + 1, r)
    inversions = CROSS-INVERSION-COUNT(A, p, q, r) + left + right
    return inversions
```

