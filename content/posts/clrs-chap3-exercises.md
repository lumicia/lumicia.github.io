---
title: "算法导论 Chap3 习题"
date: 2021-04-07T09:33:29+08:00
categories: ["Algorithm"]
tags: ["CLRS"]
draft: false
---

## 3.1

### 3.1-1

因为 $f(n)$ 和 $g(n)$ 都是渐近非负的，所以对于 $f(n)$ 和 $g(n)$ 有：

$$
\begin{aligned}
\exists n_1:\quad f(n) & \geqslant 0\quad \text{for all}\ n > n_1 \\
\exists n_2:\quad g(n) & \geqslant 0\quad \text{for all}\ n > n_2
\end{aligned}
$$

令 $n_0 = \max(n_1, n_2)$，则对于 $n > n_0$，下列不等式成立：
$$
\begin{aligned}
f(n) & \leqslant \max(f(n), g(n)) \\
g(n) & \leqslant \max(f(n), g(n)) \\ 
\frac{1}{2}(f(n) + g(n)) & \leqslant \max(f(n), g(n)) \\
\max(f(n), g(n)) & \leqslant (f(n) + g(n))
\end{aligned}
$$
结合后两个不等式得：
$$
0 \leqslant \frac{1}{2}(f(n) + g(n)) \leqslant \max{(f(n), g(n))} \leqslant f(n) + g(n).
$$
即当 $c_1 = \frac{1}{2}$ 和 $c_2 = 1$ 时，$\Theta{(f(n) + g(n))}$ 的定义。

<!--more-->

### 3.1-2

将 $(n + a)^b$ 以二项式展开，得：

$$
(n + a)^b = C_0^b n^b a^0 + C_1^b n^{b - 1} a^1 + \cdots + C_b^b n^0 a^b.
$$
对二项式成立的一个不等式是对于 $x \geqslant 1$，有：

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

### 3.1-3

这个句子对于算法 $A$ 的上下界都没有准确描述。

### 3.1-4

（1）$2^{n + 1} = 2 \times 2^n$。令 $c \geqslant 2, n_0 = 0$，使得对所有 $n \geqslant n_0$ 有 $0 \leqslant 2^{n + 1} \leqslant c \times 2^n$。根据定义，$2^{n + 1} = O(2^n)$，因此为真。

（2）$2^{2n} = 2^n \times 2^n = 4^n$。无法找到任何 $c$ 和 $n_0$ 使得 $0 \leqslant 2^{2^n} = 4^n \leqslant c \times 2^n$ 对所有 $n \geqslant n_0$ 成立。

### 3.1-5

充分性：因为 $f(n) = \Theta(g(n))$，有 $0 \leqslant c_{1}g(n) \leqslant f(n) \leqslant c_{2}g(n)$ 对所有 $n \geqslant n_0$ 都成立。

由 $0 \leqslant c_{1}g(n) \leqslant f(n)$ 可得 $f(n) = O(g(n))$；由 $0 \leqslant f(n) \leqslant c_{2}g(n)$ 可得 $f(n) = \Omega(g(n))$。

必要性：因为 $f(n) = O(g(n))$ 且 $f(n) = \Omega(g(n))$，有
$$
\begin{aligned}
0 \leqslant c_{1}'g(n) \leqslant f(n)\ 对所有\ n \geqslant n_1\ 成立 \\
0 \leqslant f(n) \leqslant c_{2}'g(n)\ 对所有\ n \geqslant n_2\ 成立
\end{aligned}
$$
令 $n_0' = \max(n_1,n_2)$，合并不等式得：
$$
0 \leqslant c_{1}'g(n) \leqslant f(n) \leqslant c_{2}'g(n)\ 对所有\ n \geqslant n_0'\ 成立
$$


### 3.1-6

证明同 3.1-5。

### 3.1-7

根据定义有 $0 \leqslant o(g(n)) < ch(n)$，$0 \leqslant ch(n) < \omega(g(n))$。因此 $o(g(n)) < \omega(g(n))$，故 $o(g(n)) \cap \omega(g(n)) = \emptyes$。

### 3.1-8

$$
\begin{aligned}
\Omega(g(n,m)) = \{f(n,m): 
& 存在正常量\ c,n_0\ 和\ m_0 \\
& 使得\ 0 \leqslant cg(n,m) \leqslant f(n,m) \\
& 对所有\ n \geqslant n_0\ 或\ m \geqslant m_0\ 成立\} \\
\end{aligned}
$$

$$
\begin{aligned}
\Theta(g(n,m)) = \{f(n,m): 
& 存在正常量\ c_1,c_2,n_0\ 和\ m_0 \\
& 使得\ 0 \leqslant c_1g(n,m) \leqslant f(n,m) \leqslant c_2g(n,m) \\
& 对所有\ n \geqslant n_0\ 或\ m \geqslant m_0\ 成立\}
\end{aligned}
$$

## 3.2

### 3.2-1

（1）因为 $f(n)$ 和 $g(n)$ 都是单调递增函数，所以当 $n_1 \leqslant n_2$ 时，有 $f(n_1) \leqslant f(n_2)$ 和 $g(n_1) \leqslant g(n_2)$。另外令 $g(n_1) = m_1$， $g(n_2) = m_2$， 有 $m_1 \leqslant m_2$。综上可得：
$$
f(n_1) + g(n_1) \leqslant f(n_2) + g(n_1) \leqslant f(n_2) + g(n_2) \\
f(m_1) \leqslant f(m_2)
$$

故 $f(n) + g(n)$ 和 $f(g(n))$ 都是单调递增的。

（2）因为 $f(n)$ 和 $g(n)$ 都是非负的，所以有：
$$
f(n_1) \cdot g(n_1) \leqslant f(n_2) \cdot g(n_1) \leqslant f(n_2) \cdot g(n_2)
$$

### 3.2-2

$$
\begin{aligned}
a^{\log_{b}{c}} 
& = (c^{\log_{c}{a}})^{\log_{b}{c}} \\
& = c^{\log_{c}{a} \cdot {\log_{b}{c}}} \\
& = c^{\frac{\log_{c}{a}}{\log_{c}{b}}} \\
& = c^{\log_{b}{a}}
\end{aligned}
$$

### 3.2-3

（1）证明 $\lg(n!) = \Theta(n \lg n)$：

$$
\begin{aligned} \lg(n!) & = \lg\left(\sqrt{2\pi n}\left(\frac{n}{e}\right)^n\left(1 + \Theta\left(\frac{1}{n}\right)\right)\right) \\
& = \lg\sqrt{2\pi n } + \lg\Big(\frac{n}{e}\Big)^n + \lg\left(1+\Theta\left(\frac{1}{n}\right)\right) \\ 
& = \Theta(\sqrt n) + n\lg{\frac{n}{e}} + \lg\left(\Theta(1) + \Theta\left(\frac{1}{n}\right)\right) \\ 
& = \Theta(\sqrt n) + \Theta(n\lg n) + \Theta\left(\frac{1}{n}\right) \\ 
& = \Theta(n\lg n)
\end{aligned}
$$

（2）证明 $n! = \omega(2^n)$：
$$
\begin{aligned}
\lim_{n \to \infty} \frac{2^n}{n!} 
& = \lim_{n \to \infty} \frac{2^n}{\sqrt{2 \pi n} \left(\frac{n}{e}\right)^n \left(1 + \Theta\left(\frac{1}{n}\right)\right)} \\
& = \lim_{n \to \infty} \frac{1}{\sqrt{2 \pi n} \left(1 + \Theta\left(\frac{1}{n}\right)\right)} \left(\frac{2e}{n}\right)^n \\
& \leqslant \lim_{n \to \infty} \left(\frac{2e}{n}\right)^n \\
& \leqslant \lim_{n \to \infty} \frac{1}{2^n} = 0 & \text{for $n > 4e$}
\end{aligned}
$$
证明 $n! = o(n^n)$：
$$
\begin{aligned}
\lim_{n \to \infty} \frac{n^n}{n!} 
& = \lim_{n \to \infty} \frac{n^n}{\sqrt{2 \pi n} \left(\frac{n}{e}\right)^n \left(1 + \Theta\left(\frac{1}{n}\right)\right)} \\
& = \lim_{n \to \infty} \frac{e^n}{\sqrt{2 \pi n} \left(1 + \Theta\left(\frac{1}{n}\right)\right)} \\
& = \lim_{n \to \infty} O\left(\frac{1}{\sqrt n}\right)e^n \\
& \geqslant \lim_{n \to \infty} \frac{e^n}{c\sqrt n} & \text{(for some constant $c > 0$)} \\
& \geqslant \lim_{n \to \infty} \frac{e^n}{cn} \\
& = \lim_{n \to \infty} \frac{e^n}{c} = \infty
\end{aligned}
$$

### 3.2-4 $\star$

（1）如果 $f(n)$ 是多项式有界的，则存在常量 $c$，$k$，$n_0$ 使得对所有 $n \geqslant n_0$，有 $f(n) \leqslant cn^k$。因此 $\lg(f(n)) \leqslant kc \lg n$，即 $\lg(f(n)) = O(\lg n)$。所以如果 $\lg(f(n)) = O(\lg n)$，那么 $f(n)$ 就是多项式有界的。

因为 $\lg(n!) = \Theta(n \lg n)$，$\lceil \lg n \rceil = \Theta(\lg n)$，得：
$$
\begin{aligned}
\lg(\lceil \lg n \rceil!)
& = \Theta(\lceil \lg n \rceil \lg \lceil \lg n \rceil) \\
& = \Theta(\lg n \lg\lg n) \\
& = \omega(\lg n) \\
& \ne O(\lg n)
\end{aligned}
$$
所以 $\lceil \lg n \rceil!$ 不是多项式有界的。

（2）同样按照（1）中的分析，可得：
$$
\begin{aligned}
\lg(\lceil \lg\lg n \rceil!)
& = \Theta(\lceil \lg\lg n \rceil \lg \lceil \lg\lg n \rceil) \\
& = \Theta(\lg\lg n\lg\lg\lg n) \\
& = o((\lg\lg n)^2) \\ & = o(\lg^2(\lg n)) \\ & = o(\lg n) \\
& = O(\lg n)
\end{aligned}
$$

### 3.2-5 $\star$

$$
\begin{aligned}
\lim_{n \to \infty} \frac{\lg(\lg^*n)}{\lg^*(\lg n)}
& = \lim_{n \to \infty} \frac{\lg(\lg^* 2^n)}{\lg^*(\lg 2^n)} \\
& = \lim_{n \to \infty} \frac{\lg(1 + \lg^* n)}{\lg^* n} \\
& = \lim_{n \to \infty} \frac{\lg(1 + n)}{n} \\ & = \lim_{n \to \infty} \frac{1}{1 + n} \\
& = 0
\end{aligned}
$$

所以 $\lg^{*}(\lg n)$ 更大。

### 3.2-6

$$
\begin{aligned}
\phi^2 & = \left(\frac{1 + \sqrt 5}{2}\right)^2 = \frac{6 + 2\sqrt 5}{4} = 1 + \frac{1 + \sqrt 5}{2} = 1 + \phi \\
\hat\phi^2 & = \left(\frac{1 - \sqrt 5}{2}\right)^2 = \frac{6 - 2\sqrt 5}{4} = 1 + \frac{1 - \sqrt 5}{2} = 1 + \hat\phi
\end{aligned}
$$

### 3.2-7

基本情况：对于 $i = 0$，有：
$$
\frac{\phi^0 - \hat\phi^0}{\sqrt 5} = \frac{1 - 1}{\sqrt 5} = 0 = F_0
$$
对于 $i = 1$，有：
$$
\frac{\phi^1 - \hat\phi^1}{\sqrt 5} = \frac{(1 + \sqrt 5) - (1 - \sqrt 5)}{2 \sqrt 5} = 1 = F_1
$$
假设 $F_{i - 1} = \cfrac{\phi^{i - 1} - \hat\phi^{i - 1}}{\sqrt 5}$ 和 $F_{i - 2} = \cfrac{\phi^{i - 2} - \hat\phi^{i - 2}}{\sqrt 5}$ 成立，则：
$$
\begin{aligned}
F_i & = F_{i - 1} + F_{i - 2} \\
& = \frac{\phi^{i - 1} - \hat\phi^{i - 1}}{\sqrt 5} + \frac{\phi^{i - 2} - \hat\phi^{i - 2}}{\sqrt 5} \\
& = \frac{\phi^{i - 2}(\phi + 1) - \hat\phi^{i - 2}(\hat\phi + 1)}{\sqrt 5} \\
& = \frac{\phi^{i - 2}\phi^2 - \hat\phi^{i - 2}\hat\phi^2}{\sqrt 5} \\
& = \frac{\phi^i - \hat\phi^i}{\sqrt 5}
\end{aligned}
$$

### 3.2-8

利用 $\Theta$ 记号的对称性，可得：

$$
k \ln k = \Theta(n) \Rightarrow n = \Theta(k \ln k)
$$

则：

$$
\ln n = \Theta(\ln(k \ln k)) = \Theta(\ln k + \ln\ln k) = \Theta(\ln k)
$$

从而：

$$
\cfrac{n}{\ln n} = \cfrac{\Theta(k \ln k)}{\Theta(\ln k)} = \Theta\left({\cfrac{k \ln k}{\ln k}}\right) = \Theta(k)
$$

再利用对称性，得：

$$
\cfrac{n}{\ln n} = \Theta(k) \Rightarrow k = \Theta\left(\cfrac{n}{\ln n}\right)
$$


## 思考题

### 3-1

a. 需要选择 $c$ 使得：

$$
\sum\limits_{i = 0}^d a_i n^i = a_d n^d + a_{d - 1}n^{d - 1} + \cdots + a_1n + a_0 \leqslant cn^d
$$

不等式两边除以 $n^d$，得：
$$
c \geqslant a_d + \frac{a_{d - 1}}n + \frac{a_{d - 2}}{n^2} + \cdots + \frac{a_0}{n^d}
$$
令 $c = a_d + b$，则：
$$
\begin{aligned}
a_d + b & \geqslant a_d + \frac{a_{d - 1}}n + \frac{a_{d - 2}}{n^2} + \cdots + \frac{a_0}{n^d} \\
b & \geqslant \frac{a_{d - 1}}n + \frac{a_{d - 2}}{n^2} + \cdots + \frac{a_0}{n^d}
\end{aligned}
$$
若令 $b = 1$，则可得 $n_0$ 的值：
$$
n_0 = \max(da_{d - 1}, d\sqrt{a_{d - 2}}, \dots, d\sqrt[d]{a_0})
$$
对于 $n \geqslant n_0$：
$$
p(n) \leqslant cn^d
$$
即 $O(n^d)$ 的定义。故当 $k \geqslant d$，$p(n) = O(n^k)$。

b. 令 a 中的 $b = -1$，同理可得 $\Omega$ 记号的定义。

c. 综合 a 和 b 可得 $\Theta$ 记号的定义。

d. 同理。

e. 同理。

### 3-2

| $A$         | $B$            | $O$  | $o$  | $\Omega$ | $\omega$ | $\Theta$ |
| ----------- | -------------- | ---- | ---- | -------- | -------- | -------- |
| $\lg^k n$   | $n^{\epsilon}$ | yes  | yes  | no       | no       | no       |
| $n^k$       | $c^n$          | yes  | yes  | no       | no       | no       |
| $\sqrt{n}$  | $n^{\sin n}$   | no   | no   | no       | no       | no       |
| $2^n$       | $2^{n / 2}$    | no   | no   | yes      | yes      | no       |
| $n^{\lg c}$ | $c^{\lg n}$    | yes  | no   | yes      | no       | yes      |
| $\lg(n!)$   | $\lg(n^n)$     | yes  | no   | yes      | no       | yes      |



### 3-3

a.
$$
\begin{array}{ll} 2^{2^{n + 1}} & \\ 2^{2^n} & \\ (n + 1)! & \\ n! & \\ e^n & \\ n\cdot 2^n & \\ 2^n & \\ (3 / 2)^n & \\ (\lg n)^{\lg n} = n^{\lg\lg n} & \\ (\lg n)! & \\ n^3 & \\ n^2 = 4^{\lg n} & \\ n\lg n \text{ and } \lg(n!) & \\ n = 2^{\lg n} & \\ (\sqrt 2)^{\lg n}\quad (= \sqrt n) & \\ 2^{\sqrt{2\lg n}} & \\ \lg^2 n & \\ \ln n & \\ \sqrt{\lg n} & \\ \ln\ln n & \\ 2^{\lg^*n} & \\ \lg^*n \text{ and } \lg^*(\lg n) & \\ \lg(\lg^*n) & \\ n^{1 / \lg n}\quad (= 2) \text{ and } 1 & \end{array}
$$
b.
$$
f(n) = \begin{cases} 2^{2^{n + 2}} & \text{if $n$ is even}, \\ 0 & \text{if $n$ is odd}. \end{cases}
$$

### 3-4

a. 不成立。如 $n = O(n^2)$，但 $n^2 \ne O(n)$。

b. 不成立。如 $n^2 + n \ne \Theta(\min(n^2, n)) = \Theta(n)$。

c. 成立。因为对 $n \geqslant n_0$ 有 $f(n) \geqslant 1$。

$$
\exists c, n_0: \forall n \geqslant n_0, 0 \leqslant f(n) \leqslant cg(n)
$$

可得：

$$
0 \leqslant \lg f(n) \leqslant \lg(cg(n)) = \lg c + \lg(g(n))
$$

现在需要证明存在常量 $c'$ 使得 $\lg(f(n)) \leqslant c' \lg(g(n))$。

因为 $\lg(g(n)) \geqslant 1$，所以：
$$
c' = \frac{\lg c + \lg(g(n))}{\lg(g(n))} = \frac{\lg c}{\lg(g(n))} + 1 \leqslant \lg c + 1
$$
常量 $c'$ 存在，故命题成立。

d. 不成立。如 $2n = O(n)$，但 $2^{2n} = 4^n \ne O(2^n)$。

e. 成立。当 $f(n) \geqslant 1$ 时，对所有 $n \geqslant 1$ 来说 $0 \leqslant f(n) \leqslant cf^2(n)$ 是平凡的。

f. 成立。首先有 $0 \leqslant f(n) \leqslant cg(n)$，需要证明存在常量 $c'$ 使得 $0 \leqslant c' f(n) \leqslant g(n)$。当 $c' = \cfrac{1}{c}$ 时明显成立。

g. 不成立。令 $f(n) = 2^n$，则需要证明下面的不等式成立：

$$
\exists c_1, c_2, n_0: \forall n \geqslant n_0, 0 \leqslant c_1 \cdot 2^{n / 2} \leqslant 2^n \leqslant c_2 \cdot 2^{n / 2}
$$

不等式同除以 $2^{n / 2}$，得：
$$
c_1 \leqslant 2^{n / 2} \leqslant c_2
$$
因为 $c_2$ 为常量，对任意大的 $n$，不等式不可能成立。

h. 成立。令 $g(n) = o(f(n))$，有：
$$
\exists c, n_0: \forall n \geqslant n_0, 0 \leqslant g(n) < cf(n)
$$
需要证明：
$$
\exists c_1, c_2, n_0: \forall n \geqslant n_0, 0 \leqslant c_1f(n) \leqslant f(n) + g(n) \leqslant c_2f(n)
$$
令 $c_1 = 1$，$c_2 = c + 1$，不等式成立。

### 3-5

a. 
$$
f(n) = \begin{cases}
O(g(n)) \text{ and } \mathop \Omega \limits^\infty(g(n)) & \text{if $f(n) = \Theta(g(n))$}, \\
O(g(n)) & \text{if $0 \leqslant f(n) \leqslant cg(n)$}, \\
\mathop \Omega \limits^\infty(g(n)) & \text{if $0 \leqslant cg(n) \leqslant f(n)$, 对无穷多的整数 $n$}.
\end{cases}
$$
b. 优点：可以刻画所有函数之间的关系。

缺点：刻画不够精确。

c. 对任意两个函数 $f(n)$ 和 $g(n)$，如果 $f(n) = \Theta(g(n))$，有：
$$
\begin{aligned}
f(n) & = O'(g(n)) \\
f(n) & = \Omega(g(n)) \\
\end{aligned}
$$

反之则不成立。

d. $\tilde\Omega$ 记号的定义：
$$
\begin{aligned}
\tilde\Omega(g(n)) = \{f(n): &\ 存在正常量\ c, k\ 和\ n_0\ 使得对所有\ n \geqslant n_0\ 有 \\
& 0 \leqslant  cg(n)\lg^k(n) \leqslant f(n)\}
\end{aligned}
$$
$\tilde\Theta$ 记号的定义：
$$
\begin{aligned}
\tilde{\Theta}(g(n)) = \{f(n): &\ 存在正常量\ c_1, c_2, k_1, k_2\ 和\ n_0\ 使得对所有\ n \geqslant n_0\ 有 \\
& 0 \leqslant c_1 g(n) \lg^{k_1}(n) \leqslant f(n) \leqslant c_2g (n) \lg^{k_2}(n) \}
\end{aligned}
$$
对任意两个函数 $f(n)$ 和 $g(n)$，有：
$$
f(n) = \tilde\Theta(g(n))
$$
当且仅当：
$$
f(n) = \tilde O(g(n))\quad \text{and}\quad f(n) = \tilde\Omega(g(n))
$$

### 3-6

| $f(n)$      | $c$  | $f_c^*(n)$                      |
| ----------- | ---- | ------------------------------- |
| $n-1$       | 0    | $\Theta(n)$                     |
| $\lg n$     | 1    | $\Theta(\lg^* n)$               |
| $n / 2$     | 1    | $\Theta(\lg n)$                 |
| $n / 2$     | 2    | $\Theta(\lg n)$                 |
| $\sqrt n$   | 2    | $\Theta(\lg \lg n)$             |
| $\sqrt n$   | 1    | NA                              |
| $n^{1 / 3}$ | 2    | $\Theta(\log_3 \lg n)$          |
| $n / \lg n$ | 2    | $\omega(\lg \lg n)$，$o(\lg n)$ |

参考答案：https://github.com/walkccc/CLRS。
