---
title: "Fluent Python Chap4：文本与字节"
date: 2021-05-05T10:38:00+08:00
categories: ["Python"]
tags: ["Fluent Python"]
draft: false
---

## 字符问题

「字符串」的概念可简单表述为字符的序列。问题在于「字符」的定义。

而目前「字符」的最佳定义是 Unicode 字符。从 Python 3 中 `str` 中获得的项是 Unicode 字符。

<!--more-->

Unicode 标准明确区分了字符的标识和具体的字节表示：

- 字符的标识称为码位（code point）， 是一个十进制数字，值从 `0` 到 `1114111`。在 Unicode 标准中表示为 4 到 6 个十六进制数字，并加上前缀 `U+`，值从 `U+0000` 到 `U+10FFFF`。
- 实际代表一个字符的字节取决于所使用的编码（encode）。编码是种将码位转换成字节序列的算法。

将码点转换成字节的过程称为编码（encoding），将字节转换成码位的过程称为解码（decoding）。

```python
s = 'café'
len(s)            # 4
b = s.encode('utf8')
b                 # b'caf\xc3\xa9'
len(b)            # 5
b.decode('utf8')  # 'café'
```

## 字节基本知识

有两种二进制序列：

- 不可变的 `bytes` 类型
- 可变的 `bytearray` 类型

`bytes` 和 `bytearray` 中的每个项都是从 `0` 到 `255` 的整数。

```python
cafe = bytes('café', encoding='utf_8')
cafe           # b'caf\xc3\xa9'
cafe[0]        # 99
cafe[:1]       # b'c'
cafe_arr = bytearray(cafe)
cafe_arr       # bytearray(b'caf\xc3\xa9')
cafe_arr[-1:]  # bytearray(b'\xa9')
```

注意 `my_bytes[0]` 会得到一个 `int`，但 `my_bytes[:1]` 则会返回一个长度为 1 的 `bytes` 对象。这对其他的序列类型几乎都成立，唯一的例外是 `str`，有 `s[0] == s[:1]`。

虽然二进制序列其实是整数序列，但是它们的字面量表示法表明其中有 ASCII 文本。因此，各个字节的值可能会使用下列三种不同的方式显示。

- 可打印的 ASCII 范围内的字节（从空格到 `~`），使用 ASCII 字符本身。
- 制表符、换行符、回车符和 \ 对应的字节，使用转义序列 `\t`、`\n`、`\r` 和 `\\`。
- 其他字节的值，使用十六进制转义序列（例如，`\x00` 是空字节）。

`bytes` 和 `bytearray` 支持绝大部分的 `str` 方法，如 `endswith`、`replace`、`strip`、`translate`、`upper`。

如果正则表达式从二进制序列而非 `str` 编译得到，则 `re` 模块中的正则表达式函数也可以在二进制序列上使用。从 Python 3.5 开始 `%` 操作符也可以处理二进制序列。

二进制序列有一个 `str` 没有的类方法 `fromhex`，可以通过解析十六进制数字对来创建二进制序列，数字对之间的空格是可选的。

```python
bytes.fromhex('31 4B CE A9')  # b'1K\xce\xa9
```

构建 `bytes` 或 `bytearray` 实例还可以调用各自的构造方法，传入下述参数。

- 一个 `str` 对象和一个 `encoding` 关键字参数。
- 一个可迭代对象，提供 `0~255` 之间的数值。
- 一个实现了缓冲协议的对象（如 `bytes`、`bytearray`、`memoryview`、`array.array`）；此时，把源对象中的字节序列复制到新建的二进制序列中。

使用类缓冲对象（buffer-like object）构建二进制序列是一种底层操作，可能涉及类型转换。

使用类缓冲对象创建 `bytes` 或 `bytearray` 对象时，始终复制源对象中的字节序列。与之相反，`memoryview` 对象允许在二进制数据结构之间共享内存。如果想从二进制序列中提取结构化信息，`struct` 模块是重要的工具。

`struct` 模块提供了一些函数，把打包的字节序列转换成不同类型字段组成的元组，还有一些函数用于执行反向转换，把元组转换成打包的字节序列。`struct` 模块能处理 `bytes`、`bytearray` 和 `memoryview` 对象。

## 基本编解码器

编解码器（codec）如 `utf_8` 有许多别名：`utf8`、`utf-8` 和 `U8` 等，可以作为 `open()`、`str.encode()`、`bytes.decode()` 等函数中 `encoding` 参数的值。

常见编码：

- `latin1`：又称为 `iso8859_1`
- `cp1252`
- `cp437`
- `gb2312`
- `utf-8`
- `utf-16le`

## 编解码问题

虽然有个一般性的 `UnicodeError` 异常，但是报告错误时几乎都会指明具体的异常：`UnicodeEncodeError`（把字符串转换成二进制序列时）或 `UnicodeDecodeError`（把二进制序列转换成字符串时）。如果源码的编码与预期不符，加载 Python 模块时还可能抛出 `SyntaxError`。

### 处理 `UnicodeEncodeError`

多数非 UTF 编解码器只能处理 Unicode 字符的一小部分子集。把文本转换成字节序列时，如果目标编码中没有定义某个字符，那就会抛出 `UnicodeEncodeError` 异常，除非把 `errors` 参数传给编码方法或函数，对错误进行特殊处理。

```python
city = 'São Paulo'
city.encode('utf_8')      # b'S\xc3\xa3o Paulo'
city.encode('utf_16')     # b'\xff\xfeS\x00\xe3\x00o\x00 \x00P\x00a\x00u\x00l\x00o\x00'
city.encode('iso8859_1')  # b'S\xe3o Paulo'
city.encode('cp437')      # UnicodeEncodeError: 'charmap' codec can't encode character '\xe3' in position 1: character maps to <undefined>

city.encode('cp437', errors='ignore')  # b'So Paulo'
city.encode('cp437', errors='replace') # b'S?o Paulo'
city.encode('cp437', errors='xmlcharrefreplace') # b'S&#227;o Paulo'
```

指定 `errors='ignore'` 会跳过无法编码的字符，但通常是个坏主意。

指定 `errors='replace'` 会把无法编码的字符替换成 `'?'`，数据损坏了，但是用户知道出了问题。

指定 `errors='xmlcharrefreplace'` 会把无法编码的字符替换成 XML 实体。

编解码器的错误处理方式是可扩展的。你可以为 `errors` 参数注册额外的字符串，方法是把一个名称和一个错误处理函数传给 `codecs.register_error` 函数。

Python 3.7 引入了一个布尔方法 `str.isascii()` 来检查 Unicode 文本是否完全由 ASCII 字符组成。如果是的，那么应该可以以用任何编码器编码为字节，而不出现 `UnicodeEncodeError`。

### 处理 `UnicodeDecodeError`

不是每一个字节都包含有效的 ASCII 字符，也不是每一个字符序列都是有效的 UTF-8 或 UTF-16。因此，把二进制序列转换成文本时，如果假设是这两个编码中的一个，遇到无法转换的字节序列时会抛出 `UnicodeDecodeError`。

另一方面，很多陈旧的 8 位编码——如 `'cp1252'`、`'iso8859_1'` 和 `'koi8_r'`——能解码任何字节序列流而不抛出错误，例如随机噪声。因此，如果程序使用错误的 8 位编码，解码过程悄无声息，而得到的是无用输出。

```python
octets = b'Montr\xe9al'

octets.decode('cp1252')  # 'Montréal'
octets.decode('iso8859_7') # 'Montrιal'
octets.decode('koi8_r') # 'MontrИal'
octets.decode('utf_8') # UnicodeDecodeError: 'utf-8' codec can't decode byte 0xe9 in position 5: invalid continuation byte
octets.decode('utf_8', errors='replace') # 'Montr�al'
```

使用 `'replace'` 错误处理方式，`\xe9` 替换成了 `�`（码位是 U+FFFD），这是官方指定的 `REPLACEMENT CHARACTER`（替换字符），表示未知字符。

如果加载的 `.py` 模块中包含 UTF-8 之外的数据，而且没有声明编码，会得到类似下面的消息：

```python
SyntaxError: Non-UTF-8 code starting with '\xe1' in file ola.py on line 1, but no encoding declared; see https://python.org/dev/peps/pep-0263/ for details
```

GNU/Linux 和 OS X 系统大都使用 UTF-8，因此打开在 Windows 系统中使用 `cp1252` 编码的 `.py` 文件时可能发生这种情况。注意，这个错误在 Windows 版 Python 中也可能会发生，因为 Python 3 为所有平台设置的默认编码都是 UTF-8。

为了修正这个问题，可以在文件顶部添加一个 `coding` 注释：

```python
# coding: cp1252

print('Olá, Mundo!')
```

如何找出字节序列的编码？简单来说，不能。必须有人告诉你。

`Chardet` 包可以检测编码，使用 `chardetect` 命令。

`b'\xff\xfe'` 是 BOM，即字节序标记（byte-order mark），指明编码时使用 Intel CPU 的小端字节序。

在小字节序设备中，各个码位的最低有效字节在前面：字母 `'E'` 的码位是 `U+0045`（十进制数 69），在字节偏移的第 2 位和第 3 位编码为 69 和 0。

```python
list(u16)
# [255, 254, 69, 0, 108, 0, 32, 0, 78, 0, 105, 0, 241, 0, 111, 0]
```

在大端字节序 CPU 中，编码顺序是相反的；`'E'` 编码为 0 和 69。

为了避免混淆，UTF-16 编码在要编码的文本前面加上特殊的不可见字符 `ZERO WIDTH NO-BREAK SPACE`（U+FEFF）。在小端字节序系统中，这个字符编码为 `b'\xff\xfe'`（十进制数 255, 254）。因为按照设计，`U+FFFE` 字符不存在，在小端字节序编码中，字节序列 `b'\xff\xfe'` 必定是 `ZERO WIDTH NO-BREAK SPACE`，所以编解码器知道该用哪个字节序。

UTF-16 有两个变种：UTF-16LE，显式指明使用小端字节序； UTF-16BE，显式指明使用大端字节序。如果使用这两个变种，不会生成 BOM。

```python
u16le = 'El Niño'.encode('utf_16le')
list(u16le)
# [69, 0, 108, 0, 32, 0, 78, 0, 105, 0, 241, 0, 111, 0]

u16be = 'El Niño'.encode('utf_16be')
list(u16be)
# [0, 69, 0, 108, 0, 32, 0, 78, 0, 105, 0, 241, 0, 111]
```

如果有 BOM，UTF-16 编解码器会将其过滤掉，为你提供没有前导 `ZERO WIDTH NO-BREAK SPACE` 字符的真正文本。

根据标准，如果文件使用 UTF-16 编码，而且没有 BOM，那么应
该假定它使用的是 UTF-16BE（大端字节序）编码。然而，Intel x86 架构用的是小端字节序，因此有很多文件用的是不带 BOM 的小端字节序 UTF-16 编码。

与字节序有关的问题只对一个字（word）占多个字节的编码（如 UTF-16 和 UTF-32）有影响。UTF-8 的一大优势是，不管设备使用哪种字节序，生成的字节序列始终一致，因此不需要 BOM。

## 处理文本文件

处理文本 I/O 的最佳实践是「Unicode 三明治」：

1. 输入的 `bytes` 应该尽早解码为 `str`。
2. 三明治中间是程序的商业逻辑，只应该处理 `str` 对象文本。
3. 输出的 `str` 编码为 `bytes` 则尽可能晚。

在 Python 3 中能轻松地采纳 Unicode 三明治的建议，因为内置的 `open` 函数会在读取文件时做必要的解码，以文本模式写入文件时还会做必要的编码，所以调用 `my_file.read()` 方法得到的以及传给 `my_file.write(text)` 方法的都是字符串对象。

在 Windows 系统中如果写入文件时指定 UTF-U 编码，而读取文件时没有指定编码，Python 会使用系统默认编码 cp1252，此时可能导致解码出现乱码。

## Unicode 字符串规范化

视觉效果相同的字符可能码位序列不同，应用程序应该把它们视作相同的字符。但是，Python 看到的是不同的码位序列，因此判定二者不相等。

这个问题的解决方案是使用 `unicodedata.normalize` 函数提供的 Unicode 规范化。这个函数的第一个参数是这 4 个字符串中的一个：`'NFC'`、`'NFD'`、`'NFKC'` 和 `'NFKD'`。

安全起见，保存文本之前，最好使用 `normalize('NFC', user_text)` 清洗字符串。

大小写折叠其实就是把所有文本变成小写，再做些其他转换。这个功能由 `str.casefold()` 方法支持。

对大多数应用来说，NFC 是最好的规范化形式。不区分大小写的比较应该使用 `str.casefold()`。

## Unicode 文本排序

在 Python 中，非 ASCII 文本的标准排序方式是使用 `locale.strxfrm` 函数，根据 `locale` 模块的文档（https://docs.python.org/3/library/locale.html?highlight=strxfrm#locale.strxfrm），这个函数会“把字符串转换成适合所在区域进行比较的形式”。

`PyUCA` 库是 Unicode 排序算的纯 Python 实现。

## Unicode 数据库

Unicode 标准提供了一个完整的数据库（许多格式化的文本文件），不仅包括码位与字符名称之间的映射，还有各个字符的元数据，以及字符之间的关系。

`unicodedata` 模块中有几个函数用于获取字符的元数据。

## 支持字符串和字节序列的双模式 API

标准库中的一些函数能接受字符串或字节序列为参数，然后根据类型展现不同的行为。

如果使用字节序列构建正则表达式，`\d` 和 `\w` 等模式只能匹配 ASCII 字符；相比之下，如果是字符串模式，就能匹配 ASCII 之外的 Unicode 数字或字母。

`os` 模块中的所有函数、文件名或路径名参数既能使用字符串，也能使用字节序列。如果这样的函数使用字符串参数调用，该参数会使用 `sys.getfilesystemencoding()` 得到的编解码器自动编码，然后操作系统会使用相同的编解码器解码。

如果必须处理（也可能是修正）那些无法使用上述方式自动处理的文件名，可以把字节序列参数传给 os 模块中的函数，得到字节序列返回值。这一特性允许我们处理任何文件名或路径名，不管里面有多少乱码文字。

---

`str` 在 RAM 中存储为码位的序列，每个码位使用固定数量的字节，从而能高效直接访问任何字符或切片。

Python 3.3 之后，创建一个新的 `str` 对象时，解释器检查其中的字符，并选择适合该 `str` 最节约内存的布局：如果仅有 `latin1` 范围内的字符，`str` 会对每个码位使用一个字节；否则每个码位使用 2 或 4 个字节。