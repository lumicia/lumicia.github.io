---
title: "SQL 08：数据处理函数"
date: 2021-04-29T15:38:37+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## 函数

要注意相同功能的函数在不同 DBMS 中的名称、语法的区别。

`Convert()` 函数用于数据类型转换。

<!--more-->

### 文本处理函数

```mysql
SELECT vend_name, Upper(vend_name) AS vend_name_upcase
FROM Vendors
ORDER BY vend_name;
```

`Upper()` 函数将文本转换为大写。

函数是大小写不敏感的。

一些常用文本处理函数：

- `Left()` 返回字符串左边的部分。
- `Length()` 返回字符串长度。
- `Lower()` 将字符串转换成小写。
- `Ltrim()` 将字符串左边的空格删除。
- `Right()` 返回字符串右边的部分。
- `Rtrim()` 将字符串右边的空格删除。
- `SubSring()` 用于字符串提取。
- `Soundex()` 返回字符串的 `SOUNDEX` 值。
- `Upper()`：将字符串转换成大小。

### 日期和时间处理函数

`CurDate()` 函数用于获取当前日期。

### 数值处理函数

`Abs()` 函数返回一个数的绝对值。