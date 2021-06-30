---
title: "Fluent Python Chap16：正确重载运算符"
date: 2021-06-04T16:53:09+08:00
categories: ["Python"]
tags: ["Fluent Python"]
draft: false
---

运算符重载的作用是让用户定义的对象使用中缀运算符（如 `+` 和 `|`）或一元运算符（如 `-` 和 `~`）。宽泛地说，函数调用（`()`）、属性访问（`.`）和元素访问 / 切片（`[]`）也是运算符。

Python 对于运算符重载施加了一些限制，做好了灵活性、可用性和安全性方面的平衡：

- 不能重载内置类型的运算符；
- 不能新建运算符，只能重载现有的；
- 某些运算符不能重载——`is`、`and`、`or` 和 `not`（不过位运算符 `&`、`|` 和 `~` 可以）。

<!--more-->

三个一元运算符：

- `-`（`__neg__`）：一元取负算术运算符。
- `+`（`__pos__`）：一元取正算术运算符。
- `~`（`__invert__`）：对整数按位取反，定义为 `~x == -(x + 1)`。

内置的 `abs` 函数也被列为一元运算符，对应的特殊方法是 `__abs__`。

支持一元运算符很简单，只需实现相应的特殊方法。这些特殊方法只有一个参数，`self`。然后，使用符合所在类的逻辑实现。不过，要遵守运算符的一个基本规则：始终返回一个新对象。也就是说，不能修改 `self`，要创建并返回合适类型的新实例。

实现一元运算符和中缀运算符的特殊方法一定不能修改操作数。使用这些运
算符的表达式期待结果是新对象。只有增量赋值表达式可能会修改第一个操
作数（`self`）。

为了支持涉及不同类型的运算，Python 为中缀运算符特殊方法提供了特殊的分派机制。对表达式 `a + b` 来说，解释器会执行以下几步操作：

1. 如果 `a` 有 `__add__` 方法，而且返回值不是 `NotImplemented`，调用 `a.__add__(b)`，然后返回结果。
2. 如果 `a` 没有 `__add__` 方法，或者调用 `__add__` 方法返回 `NotImplemented`，检查 `b` 有没有 `__radd__` 方法，如果有，而且没有返回 `NotImplemented`，调用 `b.__radd__(a)`，然后返回结果。
3. 如果 `b` 没有 `__radd__` 方法，或者调用 `__radd__` 方法返回 `NotImplemented`，抛出 `TypeError`，并在错误消息中指明操作数类型不支持。

`__radd__` 是 `__add__` 的 reflected 版本或 reversed 版本。这是一种后备机制，如果左操作数没有实现 `__add__` 方法，或者实现了，但是返回 `NotImplemented` 表明它不知道如何处理右操作数，那么 Python 会调用 `__radd__` 方法。

如果一个操作数不是可迭代对象，或可迭代对象不能与向量中的浮点数元素相加，应该返回 `NotImplemented`，而不是抛出 `TypeError`。返回 `NotImplemented` 时，另一个操作数所属的类型还有机会执行运算，即 Python 会尝试调用反向方法。如果反向方法返回 `NotImplemented`，那么 Python 会抛出 `TypeError`，并返回一个标准的错误消息。

比较运算符正向和反向调用的是同一系列方法。

对 `==` 和 `!=` 来说，如果反向调用失败，Python 会比较对象的 ID，而不抛出 `TypeError`。

如果一个类没有实现就地运算符，增量赋值运算符只是语法糖：`a += b` 的
作用与 `a = a + b` 完全一样。而且，如果定义了 `__add__` 方法的话，不用编写额外的代码，`+=` 就能使用。

然而，如果实现了就地运算符方法，例如 `__iadd__`，计算 `a += b` 的结果时会调用就地运算符方法。这种运算符的名称表明，它们会就地修改左操作数，而不会创建新对象作为结果。

与 `+` 相比，`+=` 运算符对第二个操作数更宽容。`+` 运算符的两个操作数必须是相同类型，而 `+=` 的情况更明确，因为就地修改左操作数，所以结果的类型是确定的。

`__add__`：调用类的构造方法构建一个新实例，作为结果返回。

`__iadd__
`：把修改后的 self 作为结果返回。
