---
title: "SQL 04：过滤数据"
date: 2021-04-27T19:49:00+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## WHERE 子句

```mysql
SELECT prod_name, prod_price
FROM Products
WHERE prod_price = 3.49;
```

使用 `WHERE` 子句指定搜索条件来过滤数据。

<!--more-->

`WHERE` 支持的运算符：

- `=` 相等
- `<>` 不相等
- `!=` 不相等
- `<` 小于
- `<=` 小于等于
- `!<` 不小于
- `>` 大于
- `>=` 大于等于
- `!>` 不大于
- `BETWEEN AND` 在两个指定的值之间
- `IS NULL` 是否为 `NULL` 值

条件是字符串时，使用 `''` 将字符串括起来。

过滤数据时，一定要验证被过滤列中含 `NULL` 的行确实出现在返回的数据中。