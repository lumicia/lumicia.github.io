---
title: "Fluent Python Chap5：类似记录的数据结构"
date: 2021-05-10T22:04:14+08:00
categories: ["Python"]
tags: ["Fluent Python"]
draft: false
---

Python 提供了几个方式来构建仅包含几个字段的简单类，类没有或只有非常少的功能。Python 通过 `dataclass` 装饰器来支持这种称为「数据类」的模式。

<!--more-->

三个不同的类构建器来编写数据类：

- `collections.namedtuple`
- `typing.NamedTuple`：Python 3.5 中引入
- `@dataclass.dataclass`：Python 3.7 中引入

`collections.namedtuple` 和 `typing.NamedTyple` 创建 `tuple` 的子类，因此实例是不可变的。

`@dataclass` 默认得到可变的类。添加 `frozen=True` 可以让实例变成不可变的。

`typing.NamedTyple` 和 `dataclass` 支持常规的 `class` 语句语法。

具名元组的变体都提供了实例方法 `._as_dict` 从数据类实例的字段构造 `dict` 对象。`dataclass` 则提供了模块级别的函数 `dataclasses.as_dict`。

获取字段名称和默认值：

- 具名元组类的元数据位于 `._fields` 和 `._fields_defaults` 类 attribute 中；
- `dataclass` 装饰类中相同的元数据可以通过 `dataclasses` 模块的 `fields` 函数获得，返回一个 `Field` 对象的元组。

由 `typing.NamedTuple` 和 `dataclass` 定义的类，它的字段到类注解的映射存储在 `__annotations__` 类 attribute中。

实例 attribute 替换：

- 具名元组 `x` 调用 `x._replace(**kwargs)`；
- `dataclass` 装饰类调用模块级别的函数 `dataclasses.replace(x, **kwargs)`。

在运行时创建类：

- `collections.namedtuple` 和 `typing.NamedTuple` 使用默认函数调用；
- `dataclasses` 模块提供了 `make_dataclass` 函数。

## 传统具名元组

创建一个具名元组需要两个参数：

- 类名；
- 一系列字段名，可以是可迭代的多个字符串或空格隔开的单个字符串。

传入的字段值必须作为构造器的单独的位置参数。

可以通过字段名称或位置来访问字段。

## 类型化的具名元组

通过 `typing.NamedTyple` 构建的类和 `collections.namedtyple` 具有完全相同的方法，在运行时唯一的不同是出现 `__attributes__` 类字段（Python 会在运行时完全忽略它）。

## 类型注解

类型注解用于声明函数参数、返回值和变量的期望类型。

类型注解对于 Python 程序的运行时行为没有任何影响。

变量注解语法：

```python
var_name: some_type
```

`:` 后面的类型必须是以下标识符之一：

- 一个具体类；
- 一个抽象基类；
- 在 `typing` 模块定义的类型，包括特殊类型和结构体；
- 类型别名。

字段选项函数 `field()` 中的 `default_factory` 参数可以提供一个函数、类或任何其他可调用对象，每次在数据类的实例创建时，用零个参数调用来创建一个默认值。

通过 `@dataclass` 生成的 `__init__` 方法只取传给或赋值给它们的参数（如果没有则取默认值）作为实例字段中的实例 attribute。如果需要初始化实例以外还要做别的事情，可以提供一个 `__post_init__` 方法。

`__post_init__` 方法通常用于验证和计算基于其他字段的字段值。

伪类型 `typing.ClassVar` 使用 `[]` 符号来设置变量类型，并声明变量为类 attribute。

如果需要给 `__init__` 传入非实例字段的参数（称为 init-only variable），使用 `dataclasses` 模块中的伪类型 `InitVar`，语法类似于 `typing.ClassVar`。