---
title: "算法导论 Chap2 Problems"
date: 2021-06-11T22:57:53+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: false
---

> ***2-1    Insertion sort on small arrays in merge sort***
> Although merge sort runs in $\Theta(n\lg n)$ worst-case time and insertion sort runs in $\Theta(n^2)$ worst-case time, the constant factors in insertion sort can make it faster in practice for small problem sizes on many machines. Thus, it makes sense to ***coarsen*** the leaves of the recursion by using insertion sort within merge sort whensubproblems become sufficiently small. Consider a modification to merge sort in which $n/k$ sublists of length $k$ are sorted using insertion sort and then merged using the standard merging mechanism, where $k$ is a value to be determined.
> *a*. Show that insertion sort can sort the $n/k$ sublists, each of length $k$, in $\Theta(nk)$ worst-case time.
> *b*. Show how to merge the sublists in $\Theta(n\lg(n/k))$ worst-case time.
> *c*. Given that the modified algorithm runs in $\Theta(nk + n \lg(n/k))$ worst-case time, what is the largest value of $k$ as a function of n for which the modified algorithm has the same running time as standard merge sort, in terms of $\Theta$-notation?
> *d*. How should we choose $k$ in practice?

<!--more-->

a. 对长度为 $k$ 的子表使用插入排序的最坏情况需要 $\Theta(k^2)$ 时间，因此对 $n / k$ 个长度为 $k$ 的子表使用插入排序的最坏情况需要 $n / k \cdot \Theta(k^2) = \Theta(nk)$ 时间。

b. 因为有 $n / k$ 个子表，每次将两个子表合并，需要合并的次数是 $\lg (n / k)$；每次合并最坏时间为 $\Theta(n)$，所以将所有子表合并的最坏情况是 $\Theta(n \lg (n / k))$。

c. 根据题意有 $\Theta (nk + n \lg (n / k)) \leqslant \Theta(n \lg n)$。因此 $nk$ 对整体时间的影响应该小于 $ n\lg n$，$k$ 的最大值为 $\lg n$。

d. 选择让插入排序比归并排序快时的 $k$ 的值。

> ***2-2    Correctness of bubblesort***
>
> Bubblesort is a popular, but inefficient, sorting algorithm. It works by repeatedly swapping adjacent elements that are out of order.
>
> `BUBBLESORT(A)​`：
>
> ```cpp
>  for i = 1 to A.length - 1
>      for j = A.length downto i + 1
>          if A[j] < A[j - 1]
>              exchange A[j] with A[j - 1]
> ```
>
> **a.** Let $A'$ denote the output of $\text{BUBBLESORT}(A)$ To prove that $\text{BUBBLESORT}$ is correct, we need to prove that it terminates and that
>
> $$A'[1] \leqslant A'[2] \leqslant \cdots \leqslant A'[n], \tag{2.3}$$
>
> where $n = A.length$. In order to show that $\text{BUBBLESORT}$ actually sorts, what else do we need to prove?
>
> The next two parts will prove inequality $\text{(2.3)}$.
>
> **b.** State precisely a loop invariant for the **for** loop in lines 2-4, and prove that this loop invariant holds. Your proof should use the structure of the loop invariant proof presented in this chapter.
>
> **c.** Using the termination condition of the loop invariant proved in part (b), state a loop invariant for the **for** loop in lines 1-4 that will allow you to prove inequality $\text{(2.3)}$. Your proof should use the structure of the loop invariant proof presented in this chapter.
>
> **d.** What is the worst-case running time of bubblesort? How does it compare to the running time of insertion sort?

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

> ***2-3    Correctness of Horner's rule***
>
> The following code fragment implements Horner's rule for evaluating a polynomial
>
> $$ \begin{aligned} P(x) & = \sum_{k = 0}^n a_k x^k \\ & = a_0 + x(a_1 + x (a_2 + \cdots + x(a_{n - 1} + x a_n) \cdots)), \end{aligned} $$
>
> given the coefficients $a_0, a_1, \ldots, a_n$ and a value of $x$:
>
> ```
> y = 0
> for i = n downto 0
>     y = a[i] + x * y
> ```
>
> **a.** In terms of $\Theta$-notation, what is the running time of this code fragment for Horner's rule?
>
> **b.** Write pseudocode to implement the naive  polynomial-evaluation algorithm that computes each term of the polynomial from scratch. What is the running time of this algorithm? How does it compare to Horner's rule?
>
> **c.** Consider the following loop invariant:
>
> At the start of each iteration of the **for** loop of lines 2-3,
>
> $$\displaystyle y = \sum_{k = 0}^{n - (i + 1)} a_{k + i + 1} x^k.$$
>
> Interpret a summation with no terms as equaling $0$. Following the structure of the loop invariant proof presented in this chapter, use this loop invariant to show that, at termination, $\displaystyle y = \sum_{k = 0}^n  a_k x^k$.
>
> **d.** Conclude by arguing that the given code fragment correctly evaluates a polynomial characterized by the coefficients $a_0, a_1, \ldots, a_n$.

a. $\Theta(n)$。

b. `NAIVE-POLYNOMIAL-EVALUATION`

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

> ***2-4    Inversions***
>
> Let $A[1..n]$ be an array of $n$ distinct numbers. If $i < j$ and $A[i] > A[j]$, then the pair $(i, j)$ is called an ***inversion*** of $A$.
>
> **a.** List the five inversions in the array $\langle 2, 3, 8, 6, 1 \rangle$.
>
> **b.** What array with elements from the set $\{1, 2, \ldots, n\}$ has the most inversions? How many does it have?
>
> **c.** What is the relationship between the running time of insertion sort and the number of inversions in the input array? Justify your answer.
>
> **d.** Give an algorithm that determines the number of inversions in any permutation of $n$ elements in $\Theta(n\lg n)$ worst-case time. ($\textit{Hint:}$ Modify merge sort).

a. $(1, 5)$，$(2, 5)$，$(3, 4)$，$(3, 5)$，$(4, 5)$。

b. 集合 $\{1, 2, \ldots, n\}$ 的逆序构成的数组 $\langle n, n - 1, \cdots, 1\rangle$ 具有最多的逆序对。逆序对的个数为 $(n - 1) + (n - 2) + \cdots + 1 = \cfrac{n(n - 1)}{2}$。

c. 插入排序的运行时间正比于逆序对的数量。

d. 首先需要清楚逆序对的构成。如果对数组进行归并排序来计算逆序对，则数组的逆序对由子数组中的逆序对和跨子数组的逆序对两部分组成。实际只要计算跨子数组的逆序对，因为子数组中的逆序对可以通过递归计算它自己的两部分逆序对得到。

`CROSS-INVERSION-COUNT(A, p, q, r)`

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

`INVERSIONS-COUNT(A, p, r)`

```
i
if p < r
    q = floor((p + r) / 2)
    left = INVERSION-COUNT(A, p, q)
    right = INVERSION-COUNT(A, q + 1, r)
    inversions = CROSS-INVERSION-COUNT(A, p, q, r) + left + right
    return inversions
```

