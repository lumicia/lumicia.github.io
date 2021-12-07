---
title: "算法导论 Chap4：分治策略"
date: 2021-04-10T09:48:50+08:00
categories: ["Algorithm"]
tags: ["CLRS"]
draft: false
---

## 分治策略

分治策略的三个步骤：

- 分解：分解原问题为形式相同但规模更小的子问题；
- 解决：递归求解子问题，对规模足够小的子问题直接求解；
- 合并：合并子问题的解，得到原问题的解。

子问题的两种情况：

- 递归情况：当子问题足够大，需要递归求解的情况；
- 基本情况：子问题变得足够小，不再需要递归求解的情况。

对于与原问题形式不同的子问题的求解视为合并步骤的一部分。

递归式是一个等式或不等式，通过更小的输入上的函数值来描述一个函数。

例 1 归并排序的递归式：
<div>
$$
T(n) =
\begin{cases}
\Theta(1) & 若\ n = 1, \\
2T(n / 2) + \Theta(n) & 若\ n > 1.
\end{cases}
$$
</div>
递归求解可得 $T(n) = \Theta(n\lg n)$。

例 2 规模不等的子问题。如将子问题划分为 $\cfrac{2}{3}$ 和 $\cfrac{1}{3}$，假设分解和合并步骤都是线性时间的，则得到的递归式：
$$
T(n) = T(2n / 3) + T(n / 3) = \Theta(n)
$$
例 3 递归的线性查找。线性查找的递归版本仅生成一个子问题，其规模仅比原问题的规模少一个元素。每次递归调用将花费常量时间加上下一层递归调用的时间。因此递归式为：
$$
T(n) = T(n - 1) + \Theta(1) = \Theta(n)
$$
获取递归式的 $\Theta$ 或 $O$ 渐近界的三种求解方式：

- 代入法（substitution method）首先猜测一个界，然后用数学归纳法证明。
- 递归树法（recursion-tree method）将递归式转换成一棵树，以结点表示不同层次递归产生的代价。然后用界之和（bounding summation）的技术求解递归式。
- 主方法（master method）求解 $T(n) = aT(n / b) + f(n)$ 形式的递归式的界，其中 $a \geqslant 1$，$b > 1$，$f(n)$ 是给定的函数。分治算法将原问题分为 $a$ 个子问题，每个子问题都是原问题规模的 $\cfrac{1}{b}$，分解与合并步骤总共需要 $f(n)$ 时间。

当递归式是不等式时，可以用 $O$ 或 $\Omega$ 来描述递归式的解。

当声明和求解递归式时，通常忽略向上、下取整和边界条件这些对结果影响不大的细节。

## 最大子数组问题

最大子数组问题要寻找一个数组的最大子数组。

最大子数组的定义：一个数组的非空子数组，该子数组的元素之和是所有子数组中最大的。

一个数组可能有多个最大子数组。

只有数组中包含负数时，最大子数组问题才有意义。

寻找最大子数组可以采用分治策略，效仿第二章的思考题 2-4 寻找逆序对，它一定在数组 $A$ 的左半边，或右半边，或跨越数组中点。前两种情况可以递归求解，因为这两个子问题也是相同形式的最大子数组问题，只是规模更小。因此主要处理的是寻找跨越数组中点的最大子数组的情况，然后在三种情况中选取最大的那个子数组，即为 $A$ 的最大子数组。

对于跨越子数组 $A[low..high]$ 中点的最大子数组，由中点左边部分的子数组 $A[i..mid]$ 和右边部分的子数组 $A[mid+1..j]$ 组成。

```python
def find_max_crossing_subarray(
    a: list[int], low: int, mid: int, high: int
) -> tuple[int, int, int | float]:
    left_sum = float('-inf')
    sum: int = 0

    for i in range(mid, low - 1, -1):
        sum = sum + a[i]
        if sum > left_sum:
            left_sum = sum
            max_left = i

    right_sum = float('-inf')
    sum2: int = 0

    for j in range(mid + 1, high + 1):
        sum2 = sum2 + a[j]
        if sum2 > right_sum:
            right_sum = sum2
            max_right = j

    return (max_left, max_right, left_sum + right_sum)
```

`find_max_crossing_subarray` 所需时间为 $\Theta(n)$。

我们现在能够对最大子数组的三种情况进行处理了，接下来就可以定义寻找最大子数组的函数 `find_maximum_subarray`：

```python
def find_maximum_subarray(
    a: list[int], low: int, high: int
) -> tuple[int, int, int | float]:
    if high == low:
        return (low, high, a[low])
    else:
        mid = (low + high) // 2
        left_low, left_high, left_sum = find_maximum_subarray(a, low, mid)
        right_low, right_high, right_sum = find_maximum_subarray(a, mid + 1, high)
        cross_low, cross_high, cross_sum = find_max_crossing_subarray(a, low, mid, high)
        if left_sum >= right_sum and left_sum >= cross_sum:
            return (left_low, left_high, left_sum)
        elif right_sum >= left_sum and right_sum >= cross_sum:
            return (right_low, right_high, right_sum)
        else:
            return (cross_low, cross_high, cross_sum)
```

最后定义一个快捷地对整个数组寻找最大子数组的函数 `maximum_subarray`，便于测试：

```python
def maximum_subarray(a: list[int]) -> tuple[int, int, int | float]:
    return find_maximum_subarray(a, 0, len(a) - 1)
```

