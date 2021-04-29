---
title: "Sql 09：汇总数据"
date: 2021-04-29T21:35:54+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## 聚集函数

我们经常需要汇总数据而不用把它们实际检索出来。如确定表中行数、获得表中行的集合的和、找出表中列的最大、最小和平均值。

聚集函数（aggregate function）用于数据汇总，在行的集合上运算来计算和返回单个值。

```mysql
SELECT AVG(prod_price) AS avg_price
FROM Products;
```

`AVG()` 函数通过对表中行数计数并计算指定列值之和，求得该列的平均值。

```mysql
SELECT COUNT(*) AS num_cust
FROM Customers;
```

`COUNT()` 函数用于计数。

```mysql
SELECT MAX(prod_price) AS max_price
FROM Products;
```

`MAX()` 函数返回指定列的最大值。

```mysql
SELECT MIN(prod_price) AS min_price
FROM Products;
```

`MIN()` 函数返回指定列的最小值。

```mysql
SELECT SUM(quantity) AS items_ordered
FROM OrderItems
WHERE order_num = 20005;
```

`SUM()` 函数返回指定列中的所有值的和。

## 聚集不同的值

聚集函数可以：

- 对所有行执行计算，指定 `ALL` 参数或步指定参数（`ALL` 是默认行为）；
- 只包含不同的值，指定 `DISTINCT` 参数。

```mysql
SELECT AVG(DISTINCT prod_price) AS avg_price
FROM Products
WHERE vend_id = 'DLL01';
```

## 组合聚集函数

```mysql
SELECT COUNT(*) AS num_items,
       MIN(prod_price) AS price_min,
       MAX(prod_price) AS price_max,
       AVG(prod_price) AS price_avg
FROM Products;
```

可根据需要将多个聚集函数组合起来。