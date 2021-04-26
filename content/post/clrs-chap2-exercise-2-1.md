---
title: "算法导论 Chap2 练习题 2.1"
date: 2021-04-14T14:52:05+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: false
---

## 2.1-1

> Using Figure 2.2 as a model, illustrate the operation of INSERTION-SORT on the array $A = \langle 31,41,59,26,41,58\rangle$.

<!--more-->

略。

## 2.1-2

> Rewrite the INSERTION-SORT procedure to sort into nonincreasing instead of non-decreasing order.

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

## 2.1-3

> Consider the **_searching problem_**:
> **Input**: A sequence of $n$ numbers $A = \langle a_1,a_2,\dots,a_n\rangle$ and a value $v$.
> **Output**: An index $i$ such that $v=A[i]$ or the special value $\text{NIL}$ if $v$ does not appear in $A$.
> Write pseudocode for **_linear search_**, which scans through the sequence, looking for $v$. Using a loop invariant, prove that your algorithm is correct. Make sure that your loop invariant fulfills the three necessary properties.

```
for i = 1 to A.length
	if A[i] == v
		return i
return NIL
```

**Loop invariant**：每次 `for` 循环开始迭代时，子数组 $A[1..i-1]$ 由均不等于 $v$ 的元素组成。

**Initialization**：证明循环不变式在循环开始前为真。循环开始前 $i=1$。子数组为空，因此为真。

**Maintenance**：证明循环不变式在循环的下一次迭代时仍为真。将 $A[i]$ 和 $v$ 相比较，如果二者相同，就返回 $i$；否则循环继续下一次迭代。每次循环的迭代结束时，子数组 $A[1..i]$ 中不包含 $v$，因此循环不变式仍为真。在循环下一次迭代中递增 $i$ 的值将维持循环不变式。

**Termination**：证明循环不变式在循环终止时仍为真。让循环结束的条件为 $i>A.length=n$，确切地说此时必定有 $i=n+1$。将 $i$ 替换为 $n+1$，则子数组 $A[1..n]$ 由均不等于 $v$ 的元素组成，因此返回 $\text{NIL}$。观察到子数组 $A[1..n]$ 等于原来的整个数组，可以得出结论原数组中不包含等于 $v$ 的元素。故算法正确。

## 2.1-4


> Consider the problem of adding two $n$-bit binary integers, stored in two $n$-element arrays $A$ and $B$. The sum of the two integers should be stored in binary form in an $(n + 1)$-element array $C$. State the problem formally and write pseudocode for adding the two integers.

从两个都是 $n$ 位的二进制数字，得二者最左边的位数字为 1，相加后得到的数字必定是 $n+1$ 位的。于是 $C$ 的数组长度是 $n+1$。

因为涉及到进位，需要仔细考虑如何计算相加后 $C$ 中的数字。需要一个变量来保存 $A$ 和 $B$ 每个位相加时的进位。

（1）$A$ 和 $B$ 都是 1 位数字。则 $A=B=[1]$，于是 $C=[10]$。

（2）$A$ 和 $B$ 都是 2 位数字。则 $A$ 和 $B$ 可能为 $[10]$ 或 $[11]$。有三种情况：

1. $A=[10],B=[10]$，则 $C=[100]$；
2. $A=[10],B=[11]$ 或 $A=[11],B=[10]$，则 $C=[101]$；
3. $A=[11],B=[11]$，则 $C=[110]$。

可使用模运算来求余数，即 $C$ 相同位的数字；使用除法来求商，即进位的数字。

形式化描述：

**Input**: $A$ and $B$ are binary integer arrays, each of length $n$。

$\forall i\in\{1\dots n\}:\ A[i],B[i]\in\{0,1\}$.

**Output**: An binary interger array $C$，its length is $n+1$。

$\forall i\in\{1\dots n+1\}:\ C[i]\in\{0,1\}$:
$$
\sum_{i=1}^{n+1}C[i]*2^{i-1}=\sum_{j=1}^{n}A[j]*2^{j-1}+\sum_{k=1}^{n}B[k]*2^{k-1}
$$


```
let C[A.length + 1] be new array
carry = 0
for i = 1 to A.length
	// remainder
	C[i] = (A[i] + B[i] + carry) % 2
	// quotient
	carry = (A[i] + B[i] + carry) / 2
C[i + 1] = carry
return C
```

