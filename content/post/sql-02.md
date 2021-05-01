---
title: "SQL 02：检索数据"
date: 2021-04-27T15:03:34+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## SELECT 语句

`SELECT` 语句用于检索表数据。必须至少指定想选择什么和从哪儿选择。

<!--more-->

### 检索单列

```mysql
SELECT prod_name
FROM Products;
```

从表 `Products` 中选择名字为 `prod_name` 的列。

注意：

- 如果查询结果没有显式排序，则返回的数据没有特定顺序。
- 多条 SQL 语句之间用 `;` 分隔。
- SQL 语句是大小写不敏感的。为了使代码阅读和调试更加方便，所有 SQL 关键字使用大写，列和表名使用小写。
- 一条 SQL 语句中的额外空格会被忽略，因此可以分为多行，理由同上。

### 检索多列

```mysql
SELECT prod_id, prod_name, prod_price
FROM Products;
```

多个列名在 `SELECT` 后指明，列名之间用 `,` 分隔。

### 检索所有列

```mysql
SELECT *
FROM Products;
```

使用 `*` 通配符代替列名。

### 检索不同的值

```mysql
SELECT DISTINCT vend_id
FROM Products;
```

使用 `DISTINCT` 关键字来检索列中不同的值。如果有多列，则都会应用 `DISTINCT`。

### 限制结果

```mysql
SELECT prod_name
FROM Products
LIMIT 5;
```

MySQL 中使用 `LIMIT` 子句来限制检索结果。上面的代码限制返回不超过 5 行的数据。

可以使用 `OFFSET` 继续获取接下来的数据。

```mysql
SELECT prod_name
FROM Products
LIMIT 5 OFFSET 5;
```

注意：

- 第一行的索引是 `0`。

- MySQL 中语句 `LIMIT 3 OFFSET 4` 可以简写为 `LIMIT 4, 3`，前面是 `OFFSET`，后面是 `LIMIT`。

### 注释

```mysql
SELECT prod_name -- this is a comment
FROM Products;
```

使用 `--` 添加注释。

或用 `#` 作为一行开头来注释一行，用 `/* */` 来注释多行。