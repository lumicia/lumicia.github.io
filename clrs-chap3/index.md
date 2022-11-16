# 算法导论 Chap3：函数的增长


## 渐近记号

当算法的输入规模足够大时，运行时间与增长量级密切相关。此时，需要通过渐近分析来研究输入规模趋近无限时，算法的运行时间如何随输入规模的变大而增加。

渐近分析使用渐近记号描述算法的运行时间，根据定义域为自然数集的函数来定义渐近记号。有时按照各种方式活用（abuse）渐近记号更方便。渐近记号实际上应用于函数。

刻画运行时间的渐近记号应当有所区别，从而理解所指的是哪个运行时间。

- $\Theta$ 记号：给出函数的渐近上界和下界。
- $O$ 记号：给出函数的渐近上界。
- $\Omega$ 记号：给出函数的渐近下界。

### $\Theta$ 记号

对于一个给定的函数 $g(n)$，用 $\Theta(g(n))$ 表示以下 **函数的集合**：

<div>
$$
\begin{aligned}
\Theta(g(n)) = \{f(n)：存在正常量 \ c_1, c_2\ 和 \ n_0\ 使得对所有的 \ n\geqslant n_{0}\ 有 \\
0\leqslant c_{1}g(n)\leqslant f(n)\leqslant c_{2}g(n)\}.
\end{aligned}
$$
</div>

因为 $\Theta(g(n))$ 是集合，而 $f(n)$ 属于 $\Theta(g(n))$，所以可以记为 $f(n) \in \Theta(g(n))$，然后用更一般的方式记为 $f(n) = \Theta(g(n))$。

若对所有 $n\geqslant n_0$，$f(n)$ 的值在 $c_1g(n)$ 与 $c_2g(n)$ 之间，即 $f(n)$ 等于 $g(n)$ 与一个常量因子的积，则称 $g(n)$ 是 $f(n)$ 的一个渐近紧确界（asymptotically tight bound）。

$\Theta(g(n))$ 的定义要求每个成员 $f(n) \in \Theta(g(n))$ 都是渐近非负的（asymptotically nonnegative），即当 $n$ 足够大时，$f(n)$ 是非负的。因此，$g(n)$ 也必须是渐近非负的。可以进一步假设渐近记号中所有函数都是渐近非负的。

类似地，渐近正函数（asymptotically positive function）表示对所有足够大的 $n$，$f(n)$ 都为正数。

在确定渐近确界时，渐近正函数可以忽略低阶项和最高阶项的系数。因为当 $n$ 很大时，低阶项对运行时间的影响远低于最高阶项，而最高阶项的系数则只影响常量因子 $c_1$ 和 $c_2$。

$\Theta(1)$ 用来表示一个常量或关于某个变量的一个常量函数。

### $O$ 记号

$O$ 记号给出函数的渐近上界。$O(g(n))$ 的定义：

<div>
$$
O(g(n)) = \{f(n)：存在正常量 \ c\ 和 \ n_0\ 使得对所有的 \ n\geqslant n_{0}\ 有 \\ 0\leqslant f(n)\leqslant cg(n)\}.
$$
</div>

注意 $f(n) = \Theta(g(n))$ 蕴涵 $f(n) = O(g(n))$，因为 $\Theta(n) \subseteq O(n)$。

当我们说「运行时间为 $O(n^2)$ 」时，意思是存在一个 $O(n^2)$ 的函数 $f(n)$，使得对于 $n$ 的任意值，不管选择什么特定的规模为 $n$ 的输入，其运行时间的上界都是 $f(n)$。

### $\Omega$ 记号

$\Omega$ 记号给出函数的渐近下界。$\Omega(g(n))$ 的定义：
<div>
$$
\Omega(g(n)) = \{f(n)：存在正常量 \ c 和 \ n_0 使得对所有的 \ n\geqslant n_{0}\ 有 \\ 0\leqslant cg(n)\leqslant f(n)\}.
$$
</div>

### 定理 3.1

通过渐近记号的定义，可以证明以下定理：

> 对于任意两个函数 $f(n)$ 和 $g(n)$，当且仅当 $f(n) = O(g(n))$ 和 $f(n) = \Omega(g(n))$ 成立时，有 $f(n) = \Theta(g(n))$。

### 等式和不等式中的渐近记号

这一节解释等式和不等式中使用的渐近记号的含义。

$n = O(n^2)$：渐近记号独立出现在等式右边，则等于符号表示元素属于集合，即 $n \in O(n^2)$。

$2n^2 + 3n + 1 = 2n^2 + \Theta(n)$：更一般地，将公式中的渐近记号解释为匿名函数。本例中的 $f(n)$ 是集合 $\Theta(n)$ 中的某个函数。像这样使用渐近记号可以消除无关紧要的细节和杂乱的部分。

一个表达式中匿名函数的数量可以理解为渐近记号出现的次数，如 $\displaystyle\sum_{i = 1}^{n} O(i)$ 只有一个关于 $i$ 的匿名函数。

如果渐近记号出现在左边，规则是**无论怎样选择等式左边的匿名函数，总有一种方法来选择等式右边的匿名函数使等式成立**。换句话说，等式右边比左边提供的细节更粗糙。这个规则可以用来解释下面的例子。

$2n^2 + \Theta(n) = \Theta(n^2)$：对任意 $f(n) \in \Theta(n)$，存在 $g(n) \in \Theta(n^2)$ 使得 $2n^2 + f(n) = g(n)$ 对所有 $n$ 成立。

根据后面两个例子可得：
<div>
$$
\begin{aligned}
2n^2 + 3n + 1 & = 2n^2 + \Theta(n) \\
              & = \Theta(n^2).
\end{aligned}
$$
</div>

### $o$ 记号

$O$ 记号提供的上界可能不是渐近紧确的，如界限 $2n^2 = O(n^2)$ 是渐近紧确的，$2n = O(n^2)$ 是非渐近紧确的。

$o$ 记号表示非渐近紧确上界：
<div>
$$
o(g(n)) = \{f(n)：对任意正常量 \ c>0，存在常量 \ n_0\ 使得对所有的 \ n\geqslant n_{0}\ 有 \\ 0\leqslant f(n)< cg(n)\}.
$$
</div>
如 $2n = o(n^2)$，$2n^2 \ne o(n^2)$。

$O$ 记号和 $o$ 记号的主要区别是界对**某个**常量 $c > 0$ 还是对**所有**常量 $c > 0$ 成立。

当 $n$ 趋于无穷时，$f(n)$ 相对于 $g(n)$ 变得无关紧要，即：
$$
\lim_{n\to \infty}\frac{f(n)}{g(n)} = 0
$$

### $\omega$ 记号

$\omega$ 记号表示非渐近紧确下界：
<div>
$$
\omega(g(n)) = \{f(n)：对任意正常量 \ c>0，存在常量 \ n_0 使得对所有的 \ n\geqslant n_{0}\ 有 \\ 0\leqslant cg(n)< f(n)\}.
$$
</div>
如 $\cfrac{n^2}{2} = \omega(n)$，$\cfrac{n^2}{2} \ne \omega(n^2)$。

当 $n$ 趋于无穷时，有：
$$
\lim_{n\to \infty} \frac{f(n)}{g(n)} = \infty
$$

### 函数比较

假设 $f(n)$ 和 $g(n)$ 是渐近正函数，实数的许多关系性质也可以应用到渐近比较：

- 传递性：$f(n) = \Theta(g(n))$ 且 $g(n) = \Theta(h(n))$ 蕴涵 $f(n) = \Theta(h(n))$。对五种记号都成立。
- 自反性：$f(n) = \Theta(f(n))$。$f(n) = O(f(n))$。$f(n) = \Omega(f(n))$。
- 对称性：$f(n)  = \Theta(g(n))$ 当且仅当 $g(n) = \Theta(f(n))$。
- 转置对称性：$f(n) = O(n)$ 当且仅当 $g(n) = \Omega(f(n))$。$f(n) = o(n)$ 当且仅当 $g(n) = \omega(f(n))$。

$f(n)$ 和 $g(n)$ 的渐近比较与实数 $a$ 和 $b$ 的比较可以相类比：

- $f(n) = O(g(n))$ 类似于 $a\leqslant b$；
- $f(n) = \Omega(g(n))$ 类似于 $a\geqslant b$；
- $f(n) = \Theta(g(n))$ 类似于 $a = b$；
- $f(n) = o(g(n))$ 类似于 $a < b$；
- $f(n) = \omega(g(n))$ 类似于 $a > b$。

若 $f(n) = o(g(n))$，则称 $f(n)$ 渐近小于 $g(n)$；若 $f(n) = \omega(g(n))$，则称 $f(n)$ 渐近大于 $g(n)$。

渐近记号不满足实数的三分性（trichotomy）。三分性指的是对任意两个实数 $a$ 和 $b$，大于、等于和小于三种关系恰有一种必须成立。不是所有的函数都可以进行渐近比较。

## 标准记号与常用函数

### 单调性

若 $m \leqslant n$ 蕴涵 $f(m) \leqslant f(n)$，则称函数 $f(n)$ 是单调递增的。单调递减同理。

若 $m < n$ 蕴涵 $f(m) < f(n)$，则称函数 $f(n)$ 是严格递增的。严格递减同理。

### 向下取整和向上取整

$\lfloor x \rfloor$ 表示小于等于 $x$ 的最大整数。

$\lceil x \rceil$ 表示大于等于 $x$ 的最小整数。

对所有实数 $x$ 有 $x - 1 < \lfloor x \rfloor \leqslant x \leqslant \lceil x \rceil < x + 1$。

对任意整数 $n$ 有 $\lceil n / 2 \rceil + \lfloor n / 2 \rfloor = n$。

对任意实数 $x \geqslant 0$ 和整数 $a,b>0$，有：
<div>
$$
\begin{aligned}
\biggl\lfloor\frac{\lfloor x / a \rfloor}{b}\biggr\rfloor & = \biggl\lfloor\frac{x}{ab}\biggr\rfloor, \\
\biggl\lceil\frac{\lceil x / a \rceil}{b}\biggr\rceil & = \biggl\lceil\frac{x}{ab}\biggr\rceil, \\
\biggl\lceil\frac ab \biggr\rceil & \leqslant \frac{a + (b - 1)}{b}, \\
\biggl\lfloor\frac ab \biggr\rfloor & \geqslant \frac{a - (b - 1)}{b}.
\end{aligned}
$$
</div>

两个取整函数都是单调递增的。

### 模运算

对任意整数 $a$ 和任意正整数 $n$，$a \mod n = a - n \lfloor a / n \rfloor$。结果有 $0 \leqslant a \mod n < n$。

若 $(a \mod n) = (b \mod n)$，则记 $a \equiv b(\mod n)$，称模 $n$ 时 $a$ 等价于 $b$。等价地，$a \equiv b(\mod n)$ 当且仅当 $n$ 是 $b - a$ 的一个因子。

若模 $n$ 时 $a$ 不等价于 $b$，记 $a \not\equiv b(\mod n)$。

### 多项式

给定一个非负整数 $d$，$n$ 的 $d$ 次多项式 $p(n) = \displaystyle\sum_{i = 0}^{d} a_{i}n^{i}$。

其中常量 $a_0,a_1,\dots,a_d$ 是多项式的系数且 $a_d \ne 0$。

一个多项式为渐近正的当且仅当 $a_d > 0$。对于一个 $d$ 次渐近正的多项式 $p(n)$，有 $p(n) = \Theta(n^d)$。

对任意实常量 $a \geqslant 0$，函数 $n^a$ 单调递增；$a \leqslant 0$，$n^a$ 单调递减。

若对某个常量 $k$，有 $f(n) = O(n^k)$，则称函数 $f(n)$ 是多项式有界的。

### 指数

对所有实数 $a > 0$、$m$ 和 $n$，有以下恒等式：
<div>
$$
\begin{aligned}
a^0 &= 1, \\
a^1 &= a, \\
a^{-1} &= 1 / a, \\
(a^m)^n &= a^{mn}, \\
(a^m)^n &= (a^n)^m, \\
a^{m}a^{n} &= a^{m + n}.
\end{aligned}
$$
</div>

对所有 $n$ 和 $a \geqslant 1$，函数 $a^n$ 关于 $n$ 单调递增。约定 $0^0 = 1$。

对所有使 $a > 1$ 的实常量 $a$ 和 $b$，有 $\displaystyle\lim_{n \to \infty}\cfrac{n^b}{a^n} = 0$。可得 $n^b = o(a^n)$。

$e$ 表示自然对数的底，对所有实数 $x$，有：
$$
e^x = 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \cdots = \sum_{i = 0}^{\infty}\frac{x^i}{i!}
$$
对所有实数 $x$，有 $e^x \geqslant 1 + x$。仅当 $x = 0$ 时不等式的等号成立。

当 $|x| \leqslant 1$ 时，有近似估计 $1 + x \leqslant e^x \leqslant 1 + x + x^2$。

对所有 $x$，有 $\displaystyle\lim_{n \to\infty}\left(1 + \frac{x}{n}\right)^n = e^x$。

### 对数

记：
<div>
$$
\begin{aligned}
\lg n & = \log_{2}n, \\
\ln n & = \log_{e}n, \\
\lg^k n & = (\lg n)^k, \\
\lg\lg n & = \lg(\lg n).
\end{aligned}
$$
</div>
如果常量 $b > 1$，则对 $n > 0$，函数 $\log_{b}n$ 是严格递增的。

对所有实数 $n$ 和大于零的实数 $a,b,c$，且作为底时不为 1，有
<div>
$$
\begin{aligned}
a & = b^{\log_{b}a}, \\
\log_{c}(ab) & = \log_{c}a + \log_{c}b, \\
\log_{b}a^n & = n\log_{b}a, \\
\log_{b}a & = \frac{\log_{c}a}{\log_{c}b}, \\
\log_{b}(1 / a) & = -\log_{b}a, \\
\log_{b}a & = \frac{1}{\log_{a}b}, \\
a^{\log_{b}c} & = c^{\log_{b}a}.
\end{aligned}
$$
</div>
当 $|x| < 1$ 时，$\ln(1 + x)$ 的一种简单级数展开：
$$
\ln(1 + x) = x - \frac{x^2}{2} + \frac{x^3}{3} - \frac{x^4}{4} + \frac{x^5}{5} - \cdots
$$
对 $x > -1$，有 $\cfrac{x}{1 + x} \leqslant \ln(1 + x) \leqslant x$，仅当 $x = 0$ 时等式成立。

若对某个常量 $k$，$f(n) = O(\lg^{k}n)$，则称函数 $f(n)$ 是多对数有界的。

对于 $\displaystyle\lim_{n \to \infty}\cfrac{n^b}{a^n} = 0$，令 $\lg n$ 等于 $n$，$2^a$ 等于 $a$，有
$$
\lim_{n \to\infty}\frac{\lg^{b}n}{(2^a)^{\lg n}} = \lim_{n \to\infty}\frac{\lg^{b}n}{n^a} = 0
$$
可得对于任意常量 $a > 0$， 有 $\lg^{b}n = o(n^a)$。

### 阶乘

阶乘 $n!$ 定义为对整数 $n \geqslant 0$，有：
<div>
$$
n! =
\begin{cases}
1 & n = 0, \\
n \cdot (n - 1)! & n > 0.
\end{cases}
$$
</div>
阶乘函数的一个弱上界是 $n! \leqslant n^n$。

斯特林（Stirling）近似公式：
$$
n! = \sqrt{2\pi n}\left(\cfrac{n}{e}\right)^n\left(1 + \Theta\left(\cfrac{1}{n}\right)\right)
$$

可以利用斯特林公式证明：$n! = o(n^n)$，$n! = \omega(2^n)$，$\lg(n!) = \Theta(n\lg n)$。

对于 $n \geqslant 1$，有 $n! = \sqrt{2\pi n}\left(\cfrac{n}{e}\right)^n e^{\alpha_n}$，其中 $\cfrac{1}{12n + 1} < \alpha_n < \cfrac{1}{12n}$。

### 多重函数

$f^{(i)}(n)$ 表示函数 $f(n)$ 重复 $i$ 次作用于一个初值 $n$ 上。

假设 $f(n)$ 为实数集上的一个函数，对非负整数 $n$ 有：
<div>
$$
f^{(i)}(n)
\begin{cases}
n & i = 0, \\
f\left(f^{(i - 1)}(n)\right) & i > 0.
\end{cases}
$$
</div>

### 多重对数函数

$\lg^{*}n$ 表示多重对数。因为非正数的对数无定义，所以只有在 $\lg^{(i - 1)}n > 0$ 时 $\lg^{i}n$ 才有定义。

多重对数函数的定义为 $\lg^{*}n = \min\{i \geqslant 0: \lg^{(i)}n \leqslant 1\}$。

### 斐波那契数

斐波那契数的定义：
<div>
$$
\begin{aligned}
F_0 & = 0, \\
F_1 & = 1, \\
F_i & = F_{i - 1} + F_{i - 2}\quad i \geqslant 2.
\end{aligned}
$$
</div>
黄金分割率 $\phi$ 及其共轭数 $\hat\phi$ 是方程 $x^2 = x + 1$ 的两个根。
<div>
$$
\begin{aligned}
\phi & = \frac{1 + \sqrt5}{2} = 1.61803\cdots, \\
\hat\phi & = \frac{1 - \sqrt5}{2} = 0.61803\cdots.
\end{aligned}
$$
</div>
特别地，有 $F_i = \cfrac{\phi^i - \hat\phi^i}{\sqrt5}$。

因为 $|\hat\phi| < 1$，有 $\cfrac{|\hat\phi^i|}{\sqrt5} < \cfrac{1}{\sqrt5} < \cfrac{1}{2}$，其中蕴涵着 $F_i = \biggl\lfloor\cfrac{\phi^i}{\sqrt5} + \cfrac{1}{2}\biggr\rfloor$。


