---
title: "Fluent Python Chap3：字典和集合"
date: 2021-05-03T11:02:33+08:00
categories: ["Python"]
tags: ["Fluent Python"]
draft: false
---

## 映射类型的标准 API

`collections.abc` 模块中有 `Mapping` 和 `MutableMapping` 这两个抽象基类，描述了 `dict` 和其他类似的类型的接口。

<!--more-->

![Mapping](https://i.loli.net/2021/05/03/P8jZCRQ9b4cl5ou.png)

这些抽象基类的主要作用是记录和形式化映射类型的标准接口，并且它们还可以在需要支持广义上的映射类型的代码中作为 `isinstance` 的测试准则：

```python
my_dict = {}
isinstance(my_dict, abc.Mapping)         # True
isinstance(my_dict, abc.MutableMapping)  # True
```

在抽象基类上使用`isinstance`来检查，通常比检查函数参数是否为具体的`dict`类型更好，因为可能是另一种映射类型。

要实现自定义的映射类型，不要从这些抽象基类继承得到子类，而要使用 `collections.UserDict`，或通过组合来包裹 `dict`。

标准库中的 `collections.UserDict` 类和所有具体的映射类在它们的实现中封装了基本的 `dict` 类型，`dict` 则基于散列表构建。因此它们有个共同的限制，键必须是可散列的（只有键有这个要求，值并不要求是可散列的）。

> 什么是可散列的（hashable）：
>
> 如果一个对象是可散列的，那么在这个对象的生命周期中，它的散列值是不变的，而且这个对象需要实现 `__hash__()` 方法。另外可散列对象还要有 `__eq__()` 方法， 这样才能跟其他键做比较。如果两个可散列对象是相等的，那么它们的散列值一定是一样的……

数值类型和扁平不可变类型 `str` 与 `bytes` 都是可散列的。

如果容器类型是不可变的且所包含的对象也都是可散列的，则该容器类型是可散列的。

`frozenset` 永远是可散列的，因为每个所包含的元素按照定义必须是可散列的。

`tuple` 是可散列的，仅当它所有的项都是可散列的。

用户自定义类型默认是可散列的，因为散列值是其 `id()` 的返回值，且从 `object` 类继承来的 `__eq()_` 方法简单地比较对象 ID。

如果一个对象实现了自定义的 `__eq__` 方法，并且在方法中用到了这个对象的内部状态的话，那么只有当它的 `__hash__` 永远返回相同散列值的情况下，这个对象才是可散列的。实际上这要求 `__eq__()` 和 `__hash__()` 仅在对象的生命周期中使用从不改变的实例属性。

```python
a = dict(one=1, two=2, three=3)
b = {'three': 3, 'two': 2, 'one': 1}
c = dict([('two', 2), ('one', 1), ('three', 3)])
d = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
e = dict({'three': 3, 'one': 1, 'two': 2})

a == b == c == d == e  # True
```

从 Python 3.7 正式开始保留字典中键的插入顺序。因此`dict.popitem()`方法会移除并返回最后添加到`dict`中的键值对，而非原来的任意键值对。

除了`dict()`构造函数之外，也可以使用字典推导从任何以`key: value`对作为元素的可迭代对象中构建出`dict`实例。`for`语句中声明循环变量的顺序为先`key`再`value`。

`dict`最有用的两个变体：`defaultdict`和`OrderedDict`，都在`collections`模块中。

`d.update(m)`是典型的鸭子类型的范例。首先检查`m`是否有`keys`方法，如果有，则假设`m`是映射类型；否则`update()`方法回退对`m`进行迭代，假设`m`的项是`(key, value)`对。Python中大多数的映射类型的构造函数内部都使用`update()`方法的逻辑，这就意味着它们能够从其他映射类型或任何可以产生`(key, value)`对的可迭代对象初始化而来。

`setdefault()` 方法在字典项的值是可变的且需要就地更新时，避免冗余的键查询。

根据Python的快速失败哲学，用`d[k]`访问`dict`时，如果`k`键不存在，抛出一个异常。用`d.get(k, default)`代替`d[k]`来得到一个默认值，无论何时都比处理`KeyError`方便得多。然而，在更新找到的可变的值时，不管使用`d[k]`还是`get()`都很低效。

可以用`setdefault()`方法处理缺少的键。

## 具有弹性键查询的映射

有时候为了方便起见，就算某个键在映射里不存在，我们也希望在通过这个键读取值的时候能得到一个默认值。有两个途径能帮我们达到这个目的，一个是通过 `defaultdict` 这个类型而不是普通的 `dict`，另一个是给自己定义一个 `dict` 的子类或其他映射类型，然后在子类中实现`__missing__` 方法。

`defaultdict`用于在搜索缺少的键时按需创建项。当实例化一个`defaultdict`时，提供一个可调用对象，在传给`__getitem__`一个不存在的键参数时，产生一个默认值。

给定空`defaultdict`，创建为`dd = defaultdict(list)`，如果`'new_key'`不在`dd`中，则表达式`dd['new_key']`会进行如下步骤：

- 调用`list()`创建一个新的列表；
- 以`'new_key'`为键将列表插入到`dd`；
- 返回指向该列表的一个引用。

产生默认值的可调用对象保存在`default_factory`实例属性中。

`defaultdict`的`default_factory`只为`__getitem__`调用提供默认值。因此`dd[k]`会调用`default_factory`创建一个默认值，而`dd.get(k)`仍然返回`None`。

让`defaultdict`调用`default_factory`起作用的机制是通过`__missing__`特殊方法实现的。

映射类型能够处理缺少键的背后是`__missing__`方法。定义`dict`的子类并实现`__missing__`方法，则标准的`dict.__getitem__`方法会在找不到键时调用`__missing__`方法，而不是抛出`KeyError`。

在查询时，如果想将映射类型里的键都转换成`str`，需要在`__missing__`方法中加入`isinstance(key, str)`测试来避免递归。另外也需要实现`__contains__`方法，因为从`dict`继承而来的`__contains__`方法不会在找不到键时调用`__missing__`方法。而且实现的`__contains__`方法应该用`k in my_dict.keys()`查询而不用`k in my_dict`来避免递归。

## 字典的变种

`defaultdict` 之外的映射类型：

- `collections.OrderedDict`：保持键插入时的顺序，因为现在的`dict`也是这样，所以只用来后向兼容了。
- `collections.ChainMap`：映射的列表。按序查询映射，只要在任一映射中找到键，就表示查询成功。
- `collections.Counter`：为每个键保存一个整数计数器的映射。每次更新已存在的键，计数器就加一。

## 创建自定义映射类型

当我们需要创建自定义的映射类型时，下列映射类型不应该直接实例化，但可以子类化：

- `collections.UserDict`
- `typing.TypedDict`

## 子类化 UserDict

就创造自定义映射类型来说，以 `UserDict` 为基类，总比以普通的 `dict` 为基类要来得方便。因为`dict`有一些实现的快捷方式会最终迫使我们覆盖一些方法，而`UserDict`则可以直接继承这些方法。

`UserDict`并非继承自`dict`，而是使用组合。它具有一个内部的`dict`实例，称为`data`，保存实际的项。这可以在编写特殊方式如`__setitem__`时避免预料外的递归，并能简化`__contains__`方法的编写。

因为`UserDict`继承了`abc.MutableMapping`，自己实现的`__missing__`、`__contains__`和`__setitem__`以外的方法从`UserDict`、`MutableMapping`或`Mapping`继承。

`MutableMapping.update`方法能直接调用，也能让`__init__`使用来加载实例，实例来自其他映射、`(key, value)`对的可迭代对象以及关键字参数。因为它使用`self[key] = value`来添加项，最终会调用自己的`__setitem__`实现。

`Mapping.get`方法可以直接让自己的`get`方法继承。

## 不可变映射

标准库中的映射类型默认都是可变的，但可能会遇到需要防止意外改变映射的情况。

从 Python 3.3 起 `types` 模块提供了一个包裹类，称为 `MappingProxyType`，返回的 `mappingproxy` 实例是原始映射的一个只读动态代理。

## 字典视图

`dict` 的实例方法 `.keys()`、`.values()` 和 `.items()` 分别返回称为 `dict_keys`、`dict_values` 和 `dict_items` 的类实例。这些字典视图是 `dict` 实现中内部数据结构的只读投影。它们避免了 Python 2 中等价的方法导致的内存开销，旧的方法返回的列表会重复已经位于目标 `dict` 中的数据；另外也取代了那些返回迭代器的旧方法。

视图是可迭代的。

视图实现了`__reversed__`方法。

无法在视图中使用下标来获取项。

视图对象是动态代理。如果更新源`dict`，则可以立刻通过已存在的视图观察到改变。

`dict_keys`、`dict_values`和`dict_items`类是内含的，不能通过`__builtins__`或任何标准库模块来使用。即使得到它们中之一的引用，也不能用来重新创建一个视图。

## 集合论

集合是唯一对象的聚集。集合的一个用处是去重。

```python
l = ['spam', 'spam', 'eggs', 'spam', 'bacon', 'eggs']
set(l)        # {'eggs', 'spam', 'bacon'}
list(set(l))  # ['eggs', 'spam', 'bacon']
```

如果想要去重的同时保留每个项第一次出现的顺序，可以使用普通的 `dict` 来实现：

```python
dict.fromkeys(l).keys()      # dict_keys(['spam', 'eggs', 'bacon'])
list(dict.fromkeys(l).keys)  # ['spam', 'eggs', 'bacon']
```

集合的元素必须是可散列的。`set` 类型不是可散列的，因此不能在嵌套的`set`实例中构建一个`set`。但由于`frozenset`是可散列的，所以可以在`set`中包含`frozenset`元素。

`set` 类型实现了许多中缀运算符。

`set` 字面量形如 `{1}`、`{1, 2}`。空集没有字面量，必须写成 `set()`。`{}`是空字典。

集合字面量比构造函数（如`set([1, 2, 3])`更快且更易读。Python会对后者查询`set`名称来匹配构造函数，然后构建一个列表，最后传给构造函数，因此更慢。而对于字面量，Python运行特殊的`BUILD_SET`字节码。

`frozenset`没有字面量，必须通过调用构造函数来创建。

```python
frozenset(range(3))
# frozenset({0, 1, 2})
```

集合推导：

```python
from unicodedata import name
{chr(i) for i in range(32, 256) if 'SIGN' in name(chr(i),'')}
# {'§', '=', '¢', '#', '¤', '<', '¥', 'μ', '×', '$', '¶', '£', '©', '°', '+', '÷', '±', '>', '¬', '®', '%'}
```

可变和不可变集合许多方法是运算符重载的特殊方法。其中有些运算
符和方法会对集合做就地修改（像 `&=`、``difference_update`）。

![set](https://i.loli.net/2021/06/11/HYnuGbztILc6xvd.png)

## `dict` 和 `set` 的背后

散列表其实是一个稀疏数组（总是有空白元素的数组称为稀疏数组）。在一般的数据结构教材中，散列表里的行通常叫作表元（bucket）。在 `dict` 的散列表当中，每个键值对都占用一个表元。在64位CPU的CPython构建中，每个表元都有两个部分，一个是64位的散列值，另一个是指向元素值的64位的指针。因为表元的大小固定，所以可以通过偏移量来读取某个表元。

`hash()` 内置函数作用于所有内置类型，对于自定义类型回退调用 `__hash__`。如果两个对象在比较的时候是相等的，那它们的散列值必须相等，否则散列表算法就不能正常运行了。

同时为了让散列表高效地索引，散列值应该尽可能分散在索引空间中。因此，相似但不相等的对象应该有区别更大的散列值。

从Python 3.3 开始，当计算`str`、`bytes`和`datetime`对象的散列值时，会包括一个随机盐值（random salt value）。盐值是Python进程内的常量，但在解释器每次运行的时候都不相同。

Python 3.4 采用SipHash密码函数来计算`str`和`bytes`对象的散列值。

随机盐值和SipHash都是用于阻止DoS攻击的安全措施。

对象的散列值所含信息少于实际对象的值，因此不同对象可能有相同的散列值，这种现象称为散列冲突（hash collision）。

集合散列算法：。。。

字典散列算法：。。。

散列表给字典带来的优势和限制：

- 键必须是可散列的对象；
- 必须正确实现`__hash__` 和`__eq__`方法；
- 字典的内存开销巨大；
- 键查询很快；
- 项的顺序保留在 `entries` 表中；
- 为了节约内存，避免在 `__init__` 方法外创建实例属性。

集合类型特点：

- 集合元素必须是可散列的对象；
- 必须正确实现`__hash__`和`__eq__`方法；
- 可以很高效地判断元素是否存在于某个集合；
- 集合很消耗内存；
- 元素的次序取决于插入到集合里的次序；
- 往集合里添加元素，可能会改变集合里已有元素的次序。

字典最多使用三分之一的bucket，集合最多使用三分之二的bucket。
