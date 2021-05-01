---
title: "SQL 05：高级数据过滤"
date: 2021-04-28T09:07:15+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## AND、OR 和 IN 运算符

使用逻辑运算符精细控制过滤条件。

<!--more-->

### AND 运算符

```mysql
SELECT prod_id, prod_price, prod_name
FROM Products
WHERE vend_id = 'DLL01' AND prod_price <= 4;
```

使用 `AND` 运算符为 `WHERE` 子句添加同时满足多个过滤条件的列。

### OR 运算符

```mysql
SELECT prod_id, prod_price, prod_name
FROM Products
WHERE vend_id = 'DLL01' OR vend_id = 'BRS01';
```

使用 `OR` 运算符检索满足所有条件之一的列。

如果要结合使用 `AND` 和 `OR`，最好添加括号。

### IN 运算符

```mysql
SELECT prod_name, prod_price
FROM Products
WHERE vend_id IN ('DLL01','BRS01')
ORDER BY prod_name;
```

`IN` 运算符用于指定条件的范围，范围内的都可以匹配。

`IN` 的优点：

- 长列表中有效选项非常多时更清晰、更易于阅读。
- 与 `AND` 和 `OR` 一起使用时求值顺序更容易管理。
- 比 `OR` 更快。
- 可以包含其他 `SELECT` 语句。

### NOT 运算符

```mysql
SELECT prod_name
FROM Products
WHERE NOT vend_id = 'DLL01'
ORDER BY prod_name;
```

使用 `NOT` 否定一个条件。