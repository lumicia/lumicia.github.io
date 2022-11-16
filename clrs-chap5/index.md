# 算法导论 Chap5：概率分析和随机算法


本章讨论了当算法的运行时间不是算法重点时的分析方法。以雇用问题举例，每次对候选的应聘者进行比较，只要比上一位应聘者要好，就立即雇用，直到最后一位应聘者。我们感兴趣的是面试和雇用总共产生的费用。分析方法其实与分析算法运行时间是相同的，都是计算特定基本操作的执行次数。雇用问题算法 `HIRE-ASSISTANT(n)`：

```
best = 0
for i = 1 to n
	interview candidate i
	if candidate i is better than candidate best
		best = i
		hire candidate i
```

假定面试的费用 $c_i$ 要小于雇用的费用 $c_h$，则根据最佳应聘者在所有应聘者中的面试顺序不同，雇用的费用也不同。最坏的情况是每位应聘者都比上一位要好，最佳应聘者是最后一位面试的应聘者。最好的情况是最佳应聘者是第一位应聘者。于是我们的问题是当应聘者以随机的顺序应聘时，应当如何分析雇用费用。

我们可以使用概率分析对这个问题进行分析。概率分析对输入的分布作出假设，然后进行分析，计算出一个平均情况下的运行时间。这个时间称为算法的平均情况运行时间。

因为我们要保证应聘顺序随机，可以使用随机数生成器对应聘者的顺序进行排列。

> 随机算法的定义：行为由输入和随机数生成器产生的数值同时决定的算法。

随机算法的运行时间称为期望运行时间。

## 指示器随机变量

指示器随机变量（indicator random variable）用于概率和期望之间的转换，简化算法分析过程。

> 引理 5.1：给定一个样本空间 $S$ 和 $S$ 中的一个事件 $A$，设 $X_A = I\{A\}$，则 $E[X_A] = Pr\{A\}$。

雇用问题的期望 $E[X] = \ln n + O(1)$。

> 引理 5.2：假设应聘者以随机顺序出现，算法 HIRE-ASSISTANT 总的雇用费用在平均情况下为 $O(c_n \ln n)$。

## 随机算法

雇用问题的随机算法是确定性的，对于任何特定的输入 $n$，雇用的次数始终相等，都是 $\ln n$。并且随机方式在算法上，而非输入分布上。

雇用问题随机算法 `RANDOMIZED-HIRE-ASSISTANT(n)`：

```
randomly permute the list of candidates
best = 0
for i = i to n
	interview candidate i
	if candidate i is better than candidate best
		best = i
		hire candidate i
```

> 引理 5.3：过程 `RANDOMIZED-HIRE-ASSISTANT` 的雇用费用期望是 $O(c_h \ln n)$。

随机排列数组的两种方法：

- 为数组的每个元素 $A[i]$ 赋予一个随机的优先级 $P[i]$，然后根据优先级对数组 $A$ 中的元素进行排序。
- 原地排列给定数组。

第一种方法的排序过程 `PERMUTE-BY-SORTING(A)`：

```
n = A.length
let P[1..n] be a new array
for i = 1 to n
	P[i] = RANDOM(1, n^3)
sort A, using P as sort keys
```

> 引理 5.4：假设所有优先级都不同，则过程 `PERMUTE-BY-SORTING(A)` 产生输入的均匀随机排序。

第二种方法更好，它的原地排列过程 `RANDOMIZE-IN-PLACE(A)`：

```
n = A.length
for i = 1 to n
	swap A[i] with A[RANDOM(i, n)]
```

> 引理 5.5：过程 `RANDOMIZE-IN-PLACE` 可计算出一个均匀随机排列。

