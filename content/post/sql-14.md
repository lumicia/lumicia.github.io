---
title: "SQL 14：组合查询"
date: 2021-05-01T22:57:35+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## 组合查询

多数 SQL 查询只包含从一个或多个表中返回数据的单条 `SELECT` 语句。但是，SQL 也允许执行多个查询（多条 `SELECT` 语句），并将结果作为一个查询结果集返回。这些组合查询通常称为并（union）或复合查询（compound query）。

<!--more-->

主要有两种情况需要使用组合查询：

- 在一个查询中从不同的表返回结构数据；
- 对一个表执行多个查询，按一个查询返回数据。

### 创建组合查询

```mysql
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IL','IN','MI')
UNION
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All';
```

使用 `UNION` 操作符来创建组合查询。

### UNION 规则

进行组合时需要注意几条规则。

- `UNION` 必须由两条或两条以上的 `SELECT` 语句组成，语句之间用关键字 `UNION` 分隔（因此，如果组合四条 `SELECT` 语句，将要使用三个 `UNION` 关键字）。
- `UNION` 中的每个查询必须包含相同的列、表达式或聚集函数（不过，各个列不需要以相同的次序列出）。
- 列数据类型必须兼容：类型不必完全相同，但必须是 DBMS 可以隐含转换的类型（例如，不同的数值类型或不同的日期类型）。

### 包含或消除重复的行

```mysql
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IL','IN','MI')
UNION ALL
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All';
```

`UNION` 默认消除重复的行，`UNION ALL` 包含重复的行。

### 对组合查询结果进行排序

`SELECT` 语句的输出用 `ORDER BY` 子句排序。在用 `UNION` 组合查询时，只能使用一条 `ORDER BY` 子句，它必须位于最后一条 `SELECT` 语句之后。对于结果集，不存在用一种方式排序一部分，而又用另一种方式排序另一部分的情况，因此不允许使用多条 `ORDER BY` 子句。

```mysql
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IL','IN','MI')
UNION
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All'
ORDER BY cust_name, cust_contact;
```

