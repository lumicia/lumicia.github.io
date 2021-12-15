---
title: "算法导论 Chap4 习题"
date: 2021-04-22T09:19:36+08:00
categories: ["Algorithm"]
tags: ["CLRS"]
draft: true
---

## 4.1

### 4.1-1

既然都是负数，那么只会越加越小，所以返回数组 $A$ 的最大元素。

### 4.1-2

见 [Python 代码](https://github.com/lumicia/clrs-python/blob/master/src/chap4/exercises/exercise_4_1_2_maximum_subarray_brute_force.py)。考虑到 Python 代码的可读性与伪代码差别不大，不再~~懒得~~额外写伪代码。

### 4.1-3

略。

### 4.1-4

将算法原先返回负数和的情况修改为返回空的子数组。

### 4.1-5

这个练习暗示了使用动态规划来解决最大子数组问题。[Python 代码](https://github.com/lumicia/clrs-python/blob/master/src/chap4/exercises/exercise_4_1_5_maximum_subarray_linear_time.py) 的详细解释放在单独的文章。
