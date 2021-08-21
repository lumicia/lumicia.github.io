---
title: "Fluent Python Chap8：函数定义中的类型注解"
date: 2021-05-14T09:44:48+08:00
categories: ["Python"]
tags: ["Fluent Python"]
draft: false
---

类型注解是显式标注函数参数、返回类型、变量类型的语法。类型注解有助于代码维护、调试、CI等。

类型注解通过[PEP484](https://www.python.org/dev/peps/pep-0484/)引入的渐进式类型系统（gradual type system）实现。

通过`pip`安装`mypy`来对Python文件进行类型检查：

```bash
pip install mypy
```

渐进式类型系统的特点：

- 可选；
- 在运行时不捕获错误；
- 不会增强性能。

Mypy使用：

```bash
mypy test.py

# 标记所有没有类型注解的函数定义
mypy --disallow-untyped-defs test.py

# 标记类型注解不完整的函数定义，如只有返回值类型而没有参数类型
mypy --disallow-imcomplete-defs test.py
```

可以将上面的命令行参数加入到[配置文件](https://mypy.readthedocs.io/en/stable/config_file.html)中。如项目下的`mypy.ini`、全局的`~/.config/mypy/config`。

类型注解的基本语法：`variable: type`。

对于格式问题，可以使用`flake8`和`blue`来帮助检查。但是我遇到了`blue`依赖的兼容问题，所以只安装了`flake8`。

```bash
# 扫描整个目录
flake8 proj_name

# 扫描指定文件
flake8 proj_name/test.py

# 忽略指定文件或文件类型
flake8 proj_name/test.py --exclude
flake8 proj_name/*.py --exclude

# 忽略特定错误码，错误码之间用逗号隔开
flake8 proj_name --ignore E501

# 指定每行代码长度，默认长度 79
flake8 proj_name --max-line-length 100

# 指定特定错误码或错误类型
flake8 pro_name --select E501
flake8 pro_name --select E

# 合计扫描问题
flake8 pro_name --count

# 导出检查结果
flake8 proj_name --output-file path/to/export/result.txt
```

在使用类型注解时，对于值可能为空的参数用`Optional`指定默认值`None`，否则会被视为必需参数。

> `typing`模块中的泛型类型将在以后弃用，如`List`、`Dict`等。因此推荐使用Python 3.9 以后的版本。另外不要和抽象基类中的类型如`Mapping`等混淆。

## 在类型中使用类型注解

### `Any` 类型

`Any`类型即任何类型。对于没有标注类型的函数，类型检查器会认为参数和返回值类型是`Any`。

里氏替换原则：如果用`T2`类型的对象替换`T1`类型的对象，程序仍然正常运行，则`T2`是`T1`的子类型。

一致关系：

1. 给定`T1`和子类型`T2`，则`T2`与`T1`相一致；
2. 每个类型都与`Any`相一致；
3. `Any`与每个类型相一致。

对于类，子类和它的所有超类相一致。一个例外是`int`与`float`相一致，`float`与`complex`相一致。

### `Optional`和`Union`类型

`Optional[T]`是`Union[T, None]`的简写。

在Python 3.10 中可以用`str | int`代替`Union[str, int]`这样的写法，并且无需导入`Union`。

### 泛型集合类型

通过`container[item]`这样的泛型类型注解来标记集合类型中元素的类型。

### 元组类型

标注元组类型的三种方式：

1. 作为记录的元组。每个项与标注的类型对应。
2. 作为拥有具名字段记录的元组。因为引入的`NamedTuple`是`tuple`子类型的工厂，它所定义的具名元组和第一种方式相一致。也就是说也能用第一种方式标注类型而仍然类型安全。
3. 作为不可变序列的元组。需要指定一种类型，然后在后面加上`, ...`来表示元组用作不可变的列表。

### 泛型映射

用`MappingType[KeyType, ValueType]`标注泛型映射类型。

如果用`dict`标注，通常键的类型都是`str`，值的类型则取决于键。

### 抽象基类

在参数类型中使用抽象基类比具体类型更好，这样能接收更多类型的参数。

注意一个例外是`Number`抽象基类，它没有定义任何方法，因此没什么用。而Mypy暂时还不支持在类型注解中使用`numbers`抽象基类。

### 可迭代对象

用`Iterable[T]`标注可迭代对象类型。和`Sequence`一样，最好用于标注参数类型，而非返回值类型。用`Iterator[T]`标注返回值类型。

到Python 3.10 为止，标准库没有使用类型注解，但可以用存根文件（stub file）使类型检查器查找必需的类型注解。存根文件扩展名`.pyi`。

在参数前面加上双下划线表示它是仅限位置参数（positional-only parameter）。

在Python 3.10 用`TypeAlias`来创建类型别名：

```python
from typing import TypeAlias
another_type_name: TypeAlias = type
```

### 参数化泛型和`TypeVar`

用`TypeVar`定义类型变量：

```python
from typing import TypeVar

T = TypeVar('T')
```

通过位置参数限制类型参数：

```python
NumberT = TypeVar('NumberT', float, Decimal, Fraction)
```

对于返回值类型是抽象基类的函数，如果返回值不调用抽象基类的方法，类型检查器会报错。此时可用`bound`关键字参数来设置接收类型的上界。如下面代码中类型参数可以是`Hashable`或其子类型。

```python
HashbleT = TypeVar('HashableT', bound=Hashable)
```

其他可选参数`covariant`和`contravariant`。

`typing`模块预定义了一个`TypeVar`，名为`AnyStr`：

```python
AnyStr = TypeVar('AnyStr', bytes, str)
```

### 静态协议

`Protocol`类型定义在[PEP 544](https://www.python.org/dev/peps/pep-0544/)中，类似于其他语言的接口：协议类型定义了一个或几个方法，类型检查器会验证在实现协议类型的地方都实现了这些方法。

实现了协议的类不需要继承、注册或声明任何与定义了协议的类之间的关系。

协议相较抽象基类的一个重要优势：类型无需任何特殊声明与协议类型相一致。这让协议可以借助预先存在的类型来创建，或通过无法控制的代码所实现的类型来创建。

协议也称为静态鸭子类型。

### 回调对象

标注高阶函数返回的回调参数或函数对象：

```python
Callable[[ParamType1, ParamType2], ReturnType]
```

### NoReturn

标注从不返回的函数的返回类型，这样的函数通常会退出并抛出异常。

## 标注仅限位置参数和可变参数

`*args: T`标注任意位置参数，它们的类型都是`T`。

`**kw: T`标注任意关键字参数，它们的类型都是`dict[str, T]`。如果`kw`参数必须接收不同类型的值，使用`Union[]`或`Any`，如`**kw: Any`。

`/`记号在Python 3.8引入，用于表示之前的参数是仅限位置参数。而[PEP 484](https://www.python.org/dev/peps/pep-0484/#id38)则约定在仅限关键字参数名前加双下划线。