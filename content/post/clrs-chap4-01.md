---
title: "算法导论 Chap4 01：最大子数组问题"
date: 2021-08-21T09:48:50+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: true
---

最大子数组：数组 $A$ 的和最大的非空子数组。

只有数组中包含负数时，最大子数组问题才有意义。

寻找最大子数组可以采用分治策略，效仿第二章的思考题 2-4 寻找逆序对，它一定在数组 $A$ 的左半边，或右半边，或跨越数组中点。前两种情况可以递归求解，因为这两个子问题也是最大子数组问题，只是规模更小。因此主要是寻找跨越数组中点的最大子数组，然后在三种情况中选取最大的那个子数组，即为 $A$ 的最大子数组。

对于跨越数组中点的最大子数组 $A[low..high]$，由中点左边部分的子数组 $A[i..mid]$ 和右边部分的子数组 $A[mid+1..j]$ 组成。

`FIND-MAX-CROSSING-SUBARRAY(A, low, mid, high)`

```
left_sum = -INFINITE
sum = 0
for i = mid downto low
	sum = sum + A[i]
	if sum > left_sum
		left_sum = sum
		max_left = i
right_sum = -INFINITE
sum = 0
for j = mid + 1 to high
	sum = sum + A[j]
	if sum > right_sum
		right_sum = sum
		max_right = j
return (max_left, max_right, left_sum + right_sum)
```

`FIND-MAXIMUM-SUBARRAY(A, low, high)`

```
if high == low
	return (low, high, A[low])
else mid = floor((low + high) / 2)
	(left_low, left_high, left_sum) = FIND-MAXIMUM-SUBARRAY(A, low, mid)
	(right_low, right_high, right_sum) = FIND-MAXIMUM-SUBARRAY(A, mid + 1, high)
	(cross_low, cross_high, cross_sum) = FIND-MAXIMUM-CROSSING-SUBARRAY(A, low, high)
	if left_sum >= right_sum and left_sum >= cross_sum
		return (left_low, left_high, left_sum)
	elseif right_sum >= left_sum and right_sum >= cross_sum
		return (right_low, right_high, right_sum)
	else return (cross_low, cross_high, cross_s)
```

