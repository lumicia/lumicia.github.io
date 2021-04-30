---
title: "Sql 11：使用子查询"
date: 2021-04-30T22:33:31+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## 子查询

查询通常用于指 `SELECT` 语句。

可以在查询中嵌入查询来创建子查询。

```mysql
SELECT cust_id
FROM Orders
WHERE order_num IN (SELECT order_num
                    FROM OrderItems
                    WHERE prod_id = 'RGAN01');
```

子查询 `SELECT` 语句只能检索单个列。

使用子查询作为计算字段。

```mysql
SELECT cust_name,
       cust_state,
       (SELECT COUNT(*)
       FROM Orders
       WHERE Orders.cust_id = Customers.cust_id) AS orders
FROM Customers
ORDER BY cust_name;
```

