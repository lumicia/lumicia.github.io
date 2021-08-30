---
title: "算法导论 Chap6 练习 6.3"
date: 2021-08-23T11:13:51+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: true
---

## 6.3-1

略。

## 6.3-2

如果从下标 `1` 开始，子树不一定能维持最大堆性质，`MAX-HEAPIFY` 调用可能失败。

## 6.3-3

用数学归纳法来证明。

基本情况：证明高度 $h=0$ 时命题为真。

当 $h = 0$ 时，等价于证明最多有 $\lceil \cfrac{n}{2^{h + 1}} \rceil$ 个叶结点。由 6.1-7 知道所有叶结点下标，可算出叶结点总数为 $n\ - (\lfloor \cfrac{n}{2} \rfloor + 1) + 1 = n - \lfloor \cfrac{n}{2} \rfloor = \lceil \cfrac{n}{2} \rceil$。

归纳情况：当高度为 $h-1$ 时命题成立，证明对高度为 $h$ 时仍然成立。

设树 $T$ 在高度为 $h$ 的结点个数 $n_h$，叶结点个数 $n_0$。$T$ 删除所有叶结点得到的子树 $T'$。

$T'$ 的所有结点个数 $n' = n - n_0$。从基本情况有 $n_0 = \lceil \cfrac{n}{2} \rceil$，因此 $n' = n - \lceil \cfrac{n}{2} \rceil = \lfloor \cfrac{n}{2} \rfloor$。

$T$ 中高度为 $h$ 的结点在 $T'$ 中高度为 $h-1$，设其数量为 $n'_{h - 1}$。则 $n_h = n'_{h - 1}$。

因为对 $n'_{h - 1}$ 命题 $n'_{h - 1} \leqslant \lceil \cfrac{n'}{2^h} \rceil$ 成立，所以：
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
所以命题对 $h$ 也成立，故原命题成立。
