---
title: "Fluent Python Chap3：字典和集合"
date: 2021-05-03T11:02:33+08:00
categories: ["Python"]
tags: ["Fluent Python"]
draft: false
---

## 泛映射类型

`collections.abc` 模块中有 `Mapping` 和 `MutableMapping` 这两个抽象基类，它们的作用是为 `dict` 和其他类似的类型定义形式接口。

<!--more-->

![Mapping](https://i.loli.net/2021/05/03/P8jZCRQ9b4cl5ou.png)

这些抽象基类的主要作用是作为形式化的文档，它们定义了构建一个映射类型所需要的最基本的接口。然后它们还可以跟 `isinstance` 一起被用来判定某个数据是不是广义上的映射类型：

```python
my_dict = {}
isinstance(my_dict, abc.Mapping)         # True
isinstance(my_dict, abc.MutableMapping)  # True
```

要实现自定义的映射类型，不要从这些抽象基类继承得到子类，而要使用 `collections.UserDict`，或通过复合来包裹 `dict`。

标准库中的 `collections.UserDict` 类和所有具体的映射类在它们的实现中封装了基本的 `dict` 类型。因此它们有个共同的限制，即只有可散列的数据类型才能用作这些映射里的键（只有键有这个要求，值并不需要是可散列的数据类型）。

> 什么是可散列：
>
> 如果一个对象是可散列的，那么在这个对象的生命周期中，它的散列值是不变的，而且这个对象需要实现 `__hash__()` 方法。另外可散列对象还要有 `__eq__()` 方法， 这样才能跟其他键做比较。如果两个可散列对象是相等的，那么它们的散列值一定是一样的……

数值类型和扁平不可变类型 `str` 与 `bytes` 都是可散列的。

如果容器类型是不可变的且所包含的对象也都是可散列的，则该容器类型是可散列的。

`frozenset` 永远是可散列的，因为每个所包含的元素按照定义必须是可散列的。

如果 `tuple` 所有元素都是可散列的，则它是可散列的。

用户自定义类型默认是可散列的，因为散列值是其 `id()` 的返回值，且从 `object` 类继承来的 `__eq()_` 方法简单地比较对象 ID。

如果一个对象实现了自定义的 `__eq__` 方法，并且在方法中用到了这个对象的内部状态的话，那么只有当它的 `__hash__` 永远返回相同散列值的情况下，这个对象才是可散列的。实际上这要求 `__eq__()` 和 `__hash__()` 仅在对象的生命周期中使用从不改变的实例 attribute。

```python
a = dict(one=1, two=2, three=3)
b = {'three': 3, 'two': 2, 'one': 1}
c = dict([('two', 2), ('one', 1), ('three', 3)])
d = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
e = dict({'three': 3, 'one': 1, 'two': 2})

a == b == c == d == e  # True
```

从 Python 3.7 正式开始保留字典中键的插入顺序。

字典推导可以从任何以键值对作为元素的可迭代对象中构建出字典。

用 `setdefault` 处理找不到的键。

## 映射的弹性键查询

有时候为了方便起见，就算某个键在映射里不存在，我们也希望在通过这个键读取值的时候能得到一个默认值。有两个途径能帮我们达到这个目的，一个是通过 `defaultdict` 这个类型而不是普通的 `dict`，另一个是给自己定义一个 `dict` 的子类，然后在子类中实现`__missing__` 方法。

## 字典的变种

`defaultdict` 之外的映射类型：

- `collections.OrderedDict`
- `collections.ChainMap`
- `collections.Counter`

## 创建自定义映射类型

当我们需要创建自定义的映射类型时，下列映射类型不应该直接实例化，但可以子类化：

- `collections.UserDict`
- `typing.TypedDict`

## 子类化 UserDict

就创造自定义映射类型来说，以 `UserDict` 为基类，总比以普通的 `dict` 为基类要来得方便。

## 不可变映射

标准库中的映射类型默认都是可变的，但可能会遇到防止意外改变映射的情况。

从 Python 3.3 起 `types` 模块提供了一个包裹类，称为 `MappingProxyType`，返回的 `mappingproxy` 实例是原始映射的一个只读动态代理。

## 字典视图

`dict` 的实例方法 `.keys()`、`.values()` 和 `.items()` 分别返回称为 `dict_keys`、`dict_values` 和 `dict_items` 的类实例。

这些字典视图是 `dict` 实现中内部数据结构的只读投影。它们避免了 Python 2 中等价的方法导致的内存开销，旧的方法返回已经位于目标 `dict` 中的重复数据；另外也取代了那些返回迭代器的旧方法。

## 集合论

集合是唯一对象的聚集。

集合的一个用处是去重。

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

集合元素必须是可散列的。`set` 类型不是可散列的。

`set` 类型实现了许多中缀运算符。

`set` 字面量如 `{1}`、`{1, 2}`。空集没有字面量，必须写成 `set()`。

集合推导：

```python
from unicodedata import name
{chr(i) for i in range(32, 256) if 'SIGN' in name(chr(i),'')}
# {'§', '=', '¢', '#', '¤', '<', '¥', 'μ', '×', '$', '¶', '£', '©', '°', '+', '÷', '±', '>', '¬', '®', '%'}
```

可变和不可变集合许多方法是运算符重载的特殊方法。其中有些运算
符和方法会对集合做就地修改（像 `&=`、``difference_update`）。

## `dict` 和 `set` 的背后

散列表其实是一个稀疏数组（总是有空白元素的数组称为稀疏数组）。在一般的数据结构教材中，散列表里的行通常叫作表元（bucket）。在 `dict` 的散列表当中，每个键值对都占用一个表元，每个表元都有两个部分，一个是是散列值，另一个是指向元素值的指针。因为表元的大小固定，所以可以通过偏移量来读取某个表元。

`hash()` 内建函数作用于所有内建类型，对于自定义类型回退调用 `__hash__`。如果两个对象在比较的时候是相等的，那它们的散列值必须相等，否则散列表算法就不能正常运行了。

散列表给字典带来的优势和限制：

- 键必须是可散列的；
- 字典的内存开销巨大；
- 键查询很快；
- 在 Python 中正式将项的顺序保留在 `entries` 表中；
- 为了节约内存，避免在 `__init__` 方法外创建实例 attribute。

集合类型特点：

- 集合里的元素必须是可散列的。
- 集合很消耗内存。
- 可以很高效地判断元素是否存在于某个集合。
- 元素的次序取决于被添加到集合里的次序。
- 往集合里添加元素，可能会改变集合里已有元素的次序。

