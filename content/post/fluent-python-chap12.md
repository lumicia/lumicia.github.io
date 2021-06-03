---
title: "Fluent Python Chap12：序列的修改、散列和切片"
date: 2021-05-31T08:23:14+08:00
categories: ["Python"]
tags: ["Fluent Python"]
draft: false
---

实现行为类似不可变扁平序列一样的类。

序列类型的构造方法最好接受可迭代的对象为参数，因为所有内置的序列类型都是这样做的。

`reprlib.repr` 函数用于生成大型结构或递归结构的安全表示形式，它会限制输出字符串的长度，用 `...` 表示截断的部分。

调用 `repr()` 函数的目的是调试，因此绝对不能抛出异常。如果 `__repr__` 方法的实现有问题，那么必须处理，尽量输出有用的内容，让用户能够识别目标对象。

<!--more-->

在 Python 中创建功能完善的序列类型无需使用继承，只需实现符合序列协议的方法。

在面向对象编程中，协议是非正式的接口，只在文档中定义，在代码中不定义。Python 的序列协议只需要 `__len__` 和 `__getitem__` 两个方法。任何类只要使用标准的签名和语义实现了这两个方法，就能用在任何期待序列的地方。

协议是非正式的，没有强制力，因此如果你知道类的具体使用场景，通常只需要实现一个协议的部分。例如，为了支持迭代，只需实现 `__getitem__` 方法，没必要提供 `__len__` 方法。

添加这两个方法后，支持切片操作，但切片得到的是普通的数组。内置序列类型切片得到的是各自类型的新实例，而不是其他类型。为了得到向量类切片的向量实例，不能简单地委托给数组切片，要对 `__getitem__` 方法的参数进行适当处理。

调用 `dir(slice)`，会发现有 `start`、`stop` 和 `step` 数据属性，以及 `indices` 方法。

`indices` 方法用于优雅地处理缺失索引和负数索引，以及长度超过目标序列的切片。给定长度为 `len` 的序列，计算 `S` 表示的扩展切片的起始（`start`）和结尾（`stop`）索引，以及步幅（`stride`）。超出边界的索引会被截掉，这与常规切片的处理方式一样。这个方法会把元组中的 `start`、`stop` 和 `stride` 都变成非负数，而且都落在指定长度序列的边界内。

大量使用 `isinstance` 可能表明面向对象设计得不好，不过在 `__getitem__` 方法中使用它处理切片是合理的。在 `isinstance` 中使用抽象基类做测试能让 API 更灵活且更容易更新。

属性查找失败后，解释器会调用 `__getattr__` 方法。

仅当对象没有指定名称的属性时，Python 才会调用 `__getattr__` 方法，这是一种后备机制。为对象的属性赋值后，获取该属性不会调用 `__getattr__` 方法，解释器直接返回属性的值。

用 `__setattr__` 方法实现设置属性的逻辑。

`super()` 函数用于动态访问超类的方法，对 Python 这样支持多重继承的动态语言来说，必须能这么做。可以使用这个函数把子类方法的某些任务委托给超类中适当的方法。

多数时候，如果实现了 `__getattr__` 方法，那么也要定义 `__setattr__` 方法，以防对象的行为不一致。

`reduce()` 函数的第一个参数是接受两个参数的函数，第二个参数是一个可迭代的对象。使用 `reduce()` 函数来实现向量类的 `__hash__` 方法。