---
title: "Sql 12：联结表"
date: 2021-04-30T22:51:24+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## 联结表

联结用于在一条 `SELECT` 语句中关联表，返回一组输出。

```mysql
SELECT vend_name, prod_name, prod_price
FROM Vendors, Products
WHERE Vendors.vend_id = Products.vend_id;
```

联结多个表：

```mysql
SELECT prod_name, vend_name, prod_price, quantity
FROM OrderItems, Products, Vendors
WHERE Products.vend_id = Vendors.vend_id
AND OrderItems.prod_id = Products.prod_id
AND order_num = 20007;
```

