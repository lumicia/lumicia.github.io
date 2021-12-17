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

题目暗示了使用动态规划来解决最大子数组问题。[Python 代码](https://github.com/lumicia/clrs-python/blob/master/src/chap4/exercises/exercise_4_1_5_maximum_subarray_linear_time.py) 的详细解释放在单独的文章。

> 第 2、3、4 节习题略过。

## 4.5

### 4.5-1

a. 对于 $T(n) = 2T(\cfrac{n}{4}) + 1$，有 $a = 2$，$b = 4$，$f(n) = 1$，因此 $n^{\log_b{a}} = n^{\log_4{2}} = n^{\log_4{\sqrt 4}} = \sqrt n$ 。由于 $f(n) = O(n^{\log_4{2 - \epsilon}})$，其中 $\epsilon = 1$。应用情况 1，得 $T(n) = \Theta(\sqrt n)$。

b. 对于 $T(n) = 2T(\cfrac{n}{4}) + \sqrt n$，有 $f(n) = \sqrt n$。由于 $f(n) = \Theta(n^{\log_b a}) = \Theta(\sqrt n)$，应用情况 2，得 $T(n) = \Theta(\sqrt n \lg n)$。

c. 对于 $T(n) = 2T(\cfrac{n}{4}) + n$，有 $f(n) = n$。由于 $f(n) = \Omega(n^{\log_4{2 + \epsilon}})$，其中 $\epsilon = 1$。当 $n$ 足够大时，对于 $c = \cfrac{1}{2}$，$af(\cfrac{n}{b}) = \cfrac{n}{2} \leqslant \cfrac{n}{2}$。应用情况 3，得 $T(n) = \Theta(n)$。

d. 对于 $T(n) = 2T(\cfrac{n}{4}) + n$，有 $f(n) = n^2$。由于 $f(n) = \Omega(n^{\log_4{2 + \epsilon}})$，其中 $\epsilon = 1$。当 $n$ 足够大时，对于 $c = \cfrac{1}{2}$，$af(\cfrac{n}{b}) = \cfrac{n^2}{4} \leqslant \cfrac{n^2}{2}$。应用情况 3，得 $T(n) = \Theta(n^2)$。

### 4.5-2

对于 $T(n) = aT(n / 4) + \Theta(n^2)$，有 $b = 4$，$f(n) = \Theta(n^2)$，因此 $n^{\log_b a} = n^{\log_4 a}$。要使算法渐近快于 Strassen 算法，则：
<div>
$$
\begin{aligned}
n^{\log_4 a} &< n^{\lg 7}, \\
\log_4 a &< \lg 7.
\end{aligned}
$$
</div>
得 $a = 48$。

### 4.5-3

对于 $T(n) = T(n / 2) + \Theta(1)$，有 $a = 1$，$b = 2$，$f(n) = \Theta(1)$，因此 $n^{\log_b a} = 1$。应用情况 2，得 $T(n) = \Theta(\lg n)$。

### 4.5-4

对于 $T(n) = 4T(n / 2) + n^2\lg n$，有 $a = 4$，$b = 2$，$f(n) = n^2\lg n$，因此 $n^{\log_b a} = n^2$。对于任意正常数 $\epsilon$，比值 $\cfrac{f(n)}{n^{\log_b a}} = \cfrac{n^2\lg n}{n^2} = \lg n$ 都渐近小于 $n^{\epsilon}$，因此落入情况 2 和 3 之间的间隙。

### 4.5-5 $\star$

TODO
