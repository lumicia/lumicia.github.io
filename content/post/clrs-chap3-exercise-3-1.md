---
title: "算法导论 Chap3 练习题 3.1"
date: 2021-07-17T09:33:29+08:00
categories: ["Algorithm"]
tags: ["算法导论"]
draft: true
---

> 3.1-1
>
> Let $f(n) + g(n)$ be asymptotically nonnegative functions. Using the  basic definition of $\Theta$-notation, prove that $\max(f(n), g(n)) =  \Theta(f(n) + g(n))$.

因为 $f(n)$ 和 $g(n)$ 都是渐近非负的，所以对于 $f(n)$ 和 $g(n)$ 有：

$$
\begin{aligned}
\exists n_1, n_2: 
& f(n) \geqslant 0 & \text{for}\ n > n_1 \\
& g(n) \geqslant 0 & \text{for}\ n > n_2
\end{aligned}
$$

令 $n_0 = \max(n_1, n_2)$，则对于 $n > n_0$，下列等式为真：

$$
\begin{aligned}
f(n)             & \leqslant \max(f(n), g(n)) \\ g(n)             & \leqslant \max(f(n), g(n)) \\ (f(n) + g(n))/2  & \leqslant \max(f(n), g(n)) \\ \max(f(n), g(n)) & \leqslant (f(n) + g(n))
\end{aligned}
$$
将最后两个不等式结合：

$$
0 \leqslant \frac{f(n) + g(n)}{2} \leqslant \max{(f(n), g(n))} \leqslant f(n) + g(n).
$$
即当 $c_1 = \frac{1}{2}$ 和 $c_2 = 1$ 时，$\Theta{(f(n) + g(n))}$ 的定义。

> 3.1-2
>
> Show that for any real constants $a$ and $b$, where $b > 0$,
>
> $$(n + a)^b = \Theta(n^b). \tag{3.2}$$

将 $(n + a)^b$ 二项式展开，得：

$$
(n + a)^b = C_0^b n^b a^0 + C_1^b n^{b - 1} a^1 + \cdots + C_b^b n^0 a^b.
$$
对于 $x \geqslant 1$，有：

$$
a_0 x^0 + a_1 x^1 + \cdots + a_n x^n \leqslant (a_0 + a_1 + \cdots + a_n) x^n.
$$
因此可得：

$$
\begin{aligned}
C_0^b n^b
& \leqslant C_0^b n^b a^0 + C_1^b n^{b - 1} a^1 + \cdots + C_b^b n^0 a^b \\
& \leqslant (C_0^b + C_1^b + \cdots + C_b^b) n^b = 2^b n^b.
\end{aligned}
$$
从而 $(n + a)^b = \Theta(n^b).$

> 3.1-3
>
> Explain why the statement, "The running time of algorithm $A$ is at least $O(n^2)$," is meaningless.

对于算法 $A$ 的上下界都没有准确描述。

> 3.1-4
>
> Is $2^{n + 1} = O(2^n)$? Is $2^{2n} = O(2^n)$?

（1）$2^{n + 1} = 2 \times 2^n$。令 $c \geqslant 2, n_0 = 0$，使得对所有 $n \geqslant n_0$ 有 $0 \leqslant 2^{n + 1} \leqslant c \times 2^n$。根据定义，$2^{n + 1} = O(2^n)$，因此为真。

（2）$2^{n + 1} = 2^n \times 2^n = 4^n$。无法找到任何 $c$ 和 $n_0$ 使得 $0 \leqslant 2^{2^n} = 4^b \leqslant c \times 2^n$ 对所有 $n \geqslant n_0$ 成立。

> 3.1-5
>
> Prove Theorem 3.1.

略。