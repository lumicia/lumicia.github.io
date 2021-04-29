---
title: "Sql 10：分组数据"
date: 2021-04-29T21:54:29+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## 数据分组

使用分组可以将数据分为多个逻辑集合，对每个组进行聚集计算。

```mysql
SELECT vend_id, COUNT(*) AS num_prods
FROM Products
GROUP BY vend_id;
```

`GROUP BY` 子句用于创建分组。

## 过滤分组

```mysql
SELECT cust_id, COUNT(*) AS orders
FROM Orders
GROUP BY cust_id
HAVING COUNT(*) >= 2;
```

`HAVING` 子句用于过滤分组。

## 分组和排序

`ORDER BY` 和 `GROUP BY` 的区别：

- 前者对产生的输出排序；后者对行分组，但输出可能不是分组顺序。
- 前者任意列都可以使用；后者只能使用选择的列或表达式列，且必须使用每个选择的列表达式。
- 前者不一定需要在语句中出现；后者如果与聚集函数一起使用列，则必须使用。

## SELECT 子句顺序

| 子句     | 描述               | 要求                   |
| -------- | ------------------ | ---------------------- |
| SELECT   | 要返回的列或表达式 | 是                     |
| FROM     | 要检索的表         | 仅在从表选择数据时使用 |
| WHERE    | 行级过滤           | 否                     |
| GROUP BY | 分组说明           | 仅在按组计算聚集时使用 |
| HAVING   | 集合级过滤         | 否                     |
| ORDER BY | 输出排序顺序       | 否                     |

