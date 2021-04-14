---
title: "算法导论笔记 Chap2 03：插入排序及代码实现"
date: 2021-03-28T11:23:07+08:00
categories: ["Algorithm"]
tags: ["算法导论", "Python", "Rust"]
draft: false
---

对一个序列中的数进行排序有许多种算法，首先来看插入排序。每次排序的数称为键（key）。插入排序通过对序列中的数进行交换，将键插入到相应位置。

插入排序对数组中的数进行原地（in place）排序，任何时候最多有常数个数字存储在数组外。

插入排序的伪代码：

```
for j = 2 to A.length
	key = A[j]
	i = j - 1
	while i > 0 and A[i] > key
		A[i + 1] = A[i]
		i = i - 1
	A[i + 1] = key
```

Python 代码：

```python
def insertion_sort(a: list):
    for j in range(1, len(a)):
         key = a[j]
         i = j - 1
        
         while i >= 0 and a[i] > key:
             a[i + 1] = a[i]
             i = i - 1
                
         a[i + 1] = key
```

注意到书中数组下标从 1 到 $A.length$，而 Python 中数组下标从 0 到 $len(a)-1$。现代编程语言的数组下标都从 0 开始，因为这样计算索引偏移量不用减一，更直观。

因此循环条件做出相应调整：

- `for` 循环中 `j` 取值范围从 `1` 到 `len(a)`，左闭右开；
- `while` 循环中 `i` 应当大于等于 `0`。