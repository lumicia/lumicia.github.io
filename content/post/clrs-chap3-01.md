---
title: "算法导论 Chap3 01：渐近记号"
date: 2021-07-16T11:35:36+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: false
---

渐近记号用于描述算法的运行时间，根据定义域为自然数集的函数来定义。

有时按照各种方式活用（abuse）渐近记号是方便的。

刻画运行时间的渐近记号应当有所区别，从而理解所指的是哪个运行时间。

- $\Theta$ 记号：给出函数的渐近上界和下界。
- $O$ 记号：给出函数的渐近上界。
- $\Omega$ 记号：给出函数的渐近下界。

<!--more-->

## $\Theta$ 记号

对于一个给定的函数 $g(n)$，用 $\Theta(g(n))$ 表示以下 **函数的集合**：
$$
\Theta(g(n)) = \{f(n)：存在正常量 \ c_1, c_2\ 和 \ n_0 使得对所有的 \ n\geqslant n_{0}\ 有 \\ 0\leqslant c_{1}g(n)\leqslant f(n)\leqslant c_{2}g(n)\}
$$
因为 $\Theta(g(n))$ 是集合，$f(n)$ 属于 $\Theta(g(n))$，所以可以记为 $f(n) \in \Theta(g(n))$，然后用更一般的方式记为 $f(n) = \Theta(g(n))$。

对所有 $n\geqslant n_0$，$f(n)$ 等于 $g(n)$ 与一个常数因子的积。称 $g(n)$ 是 $f(n)$ 的一个渐近紧确界（asymptotically tight bound）。

$\Theta(g(n))$ 的定义要求每个成员 $f(n) \in \Theta(g(n))$ 都是渐近非负的（asymptotically nonnegative），即当 $n$ 足够大时，$f(n)$ 是非负的。因此，$g(n)$ 也必须是渐近非负的。

任意常量函数可以表示为 $\Theta(n^0)$ 或 $\Theta(1)$。

## $O$ 记号

$O$ 记号表示渐近上界。$O(g(n))$ 的定义：
$$
O(g(n)) = \{f(n)：存在正常量 \ c\ 和 \ n_0 使得对所有的 \ n\geqslant n_{0}\ 有 \\ 0\leqslant f(n)\leqslant cg(n)\}
$$
注意 $f(n) = \Theta(g(n))$ 蕴涵 $f(n) = O(g(n))$。

## $\Omega$ 记号

$\Omega$ 记号表示渐近下界。$\Omega(g(n))$ 的定义：
$$
\Omega(g(n)) = \{f(n)：存在正常量 \ c 和 \ n_0 使得对所有的 \ n\geqslant n_{0}\ 有 \\ 0\leqslant cg(n)\leqslant f(n)\}
$$

---

通过渐近记号的定义，可以证明以下定理：

> 对于任意两个函数 $f(n)$ 和 $g(n)$，当且仅当 $f(n) = O(g(n))$ 和 $f(n) = \Omega(g(n))$ 成立时，有 $f(n) = \Theta(g(n))$。

## 渐近记号用于等式和不等式

$n = O(n^2)$：$=$ 符号用于表示元素属于集合。

$2n^2 + 3n + 1 = 2n^2 + \Theta(n)$：$\Theta(n)$ 表示匿名函数 $f(n)$，$f(n)$ 在集合 $\Theta(n)$ 中。

$2n^2 + \Theta(n) = \Theta(n^2)$：对任意 $f(n) \in \Theta(n)$，存在 $g(n) \in \Theta(n^2)$ 使得 $2n^2 + f(n) = g(n)$ 对所有 $n$ 成立。规则是不管如何选择等式左边的匿名函数，总有一种方法来选择等式右边的匿名函数使等式成立。

根据后面两个例子可得：
$$
\begin{align}
2n^2 + 3n + 1 & = 2n^2 + \Theta(n)\\
              & = \Theta(n^2)
\end{align}
$$

## $o$ 记号

$o$ 记号表示非渐近紧确上界：
$$
o(g(n)) = \{f(n)：对任意正常量 \ c>0，存在常量 \ n_0 使得对所有的 \ n\geqslant n_{0}\ 有 \\ 0\leqslant f(n)< cg(n)\}
$$
如界限 $2n^2 = O(n^2)$ 是渐近紧确的，$2n = O(n^2)$ 是非渐近紧确的。

当 $n$ 趋于无穷时，有
$$
\lim_{n\to \infin}\frac{f(n)}{g(n)} = 0
$$

## $\omega$ 记号

$\omega$ 记号表示非渐近紧确下界：
$$
\omega(g(n)) = \{f(n)：对任意正常量 \ c>0，存在常量 \ n_0 使得对所有的 \ n\geqslant n_{0}\ 有 \\ 0\leqslant cg(n)< f(n)\}
$$
当 $n$ 趋于无穷时，有
$$
\lim_{n\to \infin} \frac{f(n)}{g(n)} = \infin
$$

## 函数比较

若 $f(n)$ 和 $g(n)$ 渐近为正，满足的关系性质：

- 传递性：$f(n) = \Theta(g(n))$ 且 $g(n) = \Theta(h(n))$ 蕴涵 $f(n) = \Theta(h(n))$。
- 自反性：$f(n) = \Theta(f(n))$。
- 对称性：$f(n)  = \Theta(g(n))$ 当且仅当 $g(n) = \Theta(f(n))$。
- 转置对称性：$f(n) = O(n)$ 当且仅当 $g(n) = \Omega(f(n))$。

不满足三分性（trichotomy）。

$f(n)$ 和 $g(n)$ 的渐近比较与实数 $a$ 和 $b$ 的比较可以相类比：

- $f(n) = O(g(n))$ 类似于 $a\leqslant b$；
- $f(n) = \Omega(g(n))$ 类似于 $a\geqslant b$；
- $f(n) = \Theta(g(n))$ 类似于 $a = b$；
- $f(n) = o(g(n))$ 类似于 $a < b$；
- $f(n) = \omega(g(n))$ 类似于 $a > b$。

