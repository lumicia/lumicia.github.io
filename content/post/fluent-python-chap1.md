---
title: "Fluent Python Chap1：Python 数据模型"
date: 2021-05-01T07:26:03+08:00
categories: ["Python"]
tags: ["Fluent Python"]
draft: false
---

数据模型其实是对 Python 框架的描述，它规范了这门语言自身构建模块的接口，这些模块包括但不限于序列、迭代器、函数、类和上下文管理器。

不管在哪种框架下写程序，都会花费大量时间去实现那些会被框架本身调用的方法，Python 也不例外。Python 解释器碰到特殊的句法时，会使用特殊方法去激活一些基本的对象操作，这些特殊方法的名字以两个下划线开头，以两个下划线结尾（例如 `__getitem__`）。对 `my_collection[key]` 求值会让解释器调用 `my_collection.__getitem__(key)`。

<!--more-->

这些特殊方法名能让你自己的对象实现和支持以下的语言构架，并与之交互：

- 迭代
- 集合类
- 属性访问
- 运算符重载
- 函数和方法的调用
- 对象的创建和销毁
- 字符串表示形式和格式化
- 管理上下文（即 `with` 块）

迭代通常是隐式的，譬如说一个集合类型没有实现 `__contains__` 方法，那么 `in` 运算符就会按顺序做一次迭代搜索。

虽然自定义的 `FrenchDeck` 类隐式继承 `object` 类，但功能却不是继承而来的。我们通过数据模型和一些合成来实现这些功能。

## 使用特殊方法

特殊方法的存在是为了被 Python 解释器调用的，你自己并不需要调用它们。应该使用 `len(my_object)`，没有 `my_object.__len__()` 这种写法。自定义 `my_object` 对象，Python 会自己去调用由你实现的 `__len__` 方法。

如果是 Python 内置的类型，比如列表（`list`）、字符串（`str`）、字节序列（`bytearray`）等，那么 CPython 会抄个近路，`__len__` 实际上会直接返回 `PyVarObject` 里的 `ob_size` 属性。`PyVarObject` 是表示内存中长度可变的内置对象的 C 语言结构体。直接读取这个值比调用一个方法要快很多。

很多时候，特殊方法的调用是隐式的，比如 `for i in x:` 这个语句，背后其实用的是 `iter(x)`，而这个函数的背后则是 `x.__iter__()` 方法。当然前提是 `x` 实现了这个方法。

除非有大量的元编程存在，直接调用特殊方法的频率应该远远低于你去实现它们的次数。唯一的例外可能是 `__init__` 方法，你的代码里可能经常会用到它，目的是在你自己的子类的 `__init__` 方法中调用超类的构造器。

通过内置的函数（例如 `len`、`iter`、`str` 等等）来使用特殊方法是最好的选择。这些内置函数不仅会调用特殊方法，通常还提供额外的好处，而且对于内置的类来说，它们的速度更快。

利用特殊方法，可以让自定义对象通过加号 `+`（或是别的运算符）进行运算。

### 字符串表示形式

Python 有一个内置的函数叫 `repr`，它能把一个对象用字符串的形式表达出来以便辨认，这就是「字符串表示形式」。`repr` 通过 `__repr__` 这个特殊方法来得到一个对象的字符串表示形式。如果没有实现 `__repr__`，当我们在控制台里打印一个向量的实例时，得到的字符串可能会是 `<Vector object at 0x10e100070>`。

交互式控制台和调试程序（debugger）对表达式的求值结果调用 `repr` 函数来获取字符串表示形式，相同做法的有用传统 `%` 操作符格式化的 `%r` 占位符，以及 f-string 和 `str.format` 方法使用的新的字符串格式语法中的 `!r` 转换字段。

`__repr__` 使用 f-string 中的 `!r` 来获得要显示的属性的标准表示形式，可以区分构造器中的参数是数值类型还是 `str` 类型。

`__repr_` 返回的字符串应该准确、无歧义，并且尽可能匹配必需的代码来重新创建出这个要表示的对象。

与 `__repr__` 相比，`__str__`  在 `str()` 构造函数中调用，并由 `print()` 函数隐式使用。它返回的字符串对终端用户更友好。

如果只实现二者之一，选择 `__repr__`。因为当没有自定义的 `__str__` 可用时，从 `object` 类继承的 `__str__` 方法会调用 `__repr__` 作为回退。

### 算术运算符

通过 `__add__` 和 `__mul__` 实现 `+` 和 `*` 运算符。值得注意的是，这两个方法的返回值都是新创建的对象，被操作的两个运算对象还是原封不动，代码里只是读取了它们的值而已。中缀运算符的基本原则就是不改变对象对象，而是产出一个新的值。

### 自定义的布尔值

尽管 Python 里有 `bool` 类型，但实际上任何对象都可以用于需要布尔值的上下文中，比如控制 `if` 或 `while` 语句的表达式，或者作为 `and`、`or` 和 `not` 的运算数。为了判定一个值 `x` 为 *truthy* 还是为 *falsy*，Python 会调用 `bool(x)`，这个函数只能返回 `True` 或者 `False`。

默认情况下，我们自己定义的类的实例总被认为是 truthy，除非这个类实现了对 `__bool__` 或者 `__len__` 之一。`bool(x)` 的背后是调用 `x.__bool__()` 的结果。如果未实现 `__bool__` 方法，那么 `bool(x)` 会尝试调用 `x.__len__()`，若返回 `0`，则 `bool` 会返回 `False`；否则返回 `True`。

下图列出了 Python 基本集合类型的接口，图中所有的类都是抽象基类（ABC，abstract base class）。斜体的方法都是抽象的，必须在具体的子类中实现（如 `list` 和 `dict`），其余的方法都有具体的实现，因此子类可以继承它们。

![collection.png](https://i.loli.net/2021/05/01/y3AQsqlY4wIRL7X.png)

顶部的每一个抽象基类都只有一个特殊方法。在 Python 3.6 中 `Collection` 抽象基类统一了三个基本接口，每个集合类型都应该实现这三个接口：

- `Iterable` 用于支持 `for` 循环、列表推导和其他形式的迭代；
- `Sized` 用于支持内置的 `len()` 函数；
- `Contains` 用于支持 `in` 操作符。

Python 并不要求具体的类实际继承这些抽象基类。任何实现了 `__len_` 方法的类都满足 `Sized` 接口。

`Collection` 中三个非常重要的特化（specialization）：

- `Sequence` 将内置类型如 `list` 和 `str` 的接口形式化；
- `Mapping` 由 `dict` 和 `collections.defaultdict` 实现；
- `Set` 是 `set` 和 `frozenset` 内置类型的接口。

只有 `Sequence` 是 `Reversible`，因为序列支持内容的任意顺序，而映射和集合不支持。

从 Python 3.7 开始，`dict` 类型是「有序的」了，但仅意味着保留键插入的顺序，不能对键任意排序。

`Set` 抽象基类除了 `isdisjoint` 以外的特殊方法都实现了中缀运算符。

[Data model ](https://docs.python.org/3/reference/datamodel.html#special-method-names) 列出了许多特殊方法的名称。

## 为何 `len` 不是一个方法

当 `x` 是内置类型的实例时，`len(x)` 非常快。因为没有调用 CPython 中内置对象的方法，长度简单地从一个 C 结构体的字段中读取。从集合类型中获取项的数量是一个常见操作，必须对基本和各种各样的类型（如 `str`、`list`、`memoryview` 等）高效执行操作。

由于运行效率原因，`len()` 并非一个方法，在一致性上做了妥协，成为 Python 数据模型中特殊的一部分，就和 `abs` 一样。但通过特殊方法 `__len__`，可以在自定义对象上使用 `len`。

通过实现特殊方法，自定义数据类型可以表现得跟内置类型一样，从而让我们写出更具表达力的代码，即 Pythonic 的代码。

Python 对象的一个基本要求就是它自身得有合理的字符串表示形式，我们可以通过 `__repr__` 和 `__str__` 来满足这个要求。前者方便我们调试和记录日志，后者则是给终端用户看的。

对序列数据类型的模拟是特殊方法用得最多的地方。

Python 通过运算符重载提供了丰富的数值类型，都支持中缀算术运算符。

作者本章结尾提到的参考书目，可以一看：

- Python in a Nutshell 3rd Edition - Alex Martelli
- Python Essential Reference 4th Edition - David Beazley
- Python Cookbook 3rd Edition - David Beazley / Brian K.Jones
- The Art of the Metaobject Protocol - Gregor Kiczales / Jim des Rivieres / Daniel G. Bobrow