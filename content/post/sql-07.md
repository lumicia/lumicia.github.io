---
title: "SQL 07：创建计算字段"
date: 2021-04-28T22:02:04+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## 计算字段

从数据库中检索直接得到的数据可能不是应用想要的，可能需要对数据进行计算、转换、或重新格式化。

此时需要计算字段来处理数据。计算字段并不存在于数据库表之中。计算字段是运行过程中在 `SELECT` 语句中创建的。

字段（field）和列（column）的意义相同。

<!--more-->

### 拼接字段

拼接（concatenate）指将多个值合并成一个较长的单个值。

```mysql
SELECT Concat(vend_name, ' (', vend_country, ')')
FROM Vendors
ORDER BY vend_name;
```

MySQL 中使用 `Concat()` 函数来拼接值。此时可能需要删除数据右侧多余的空格，可以使用 `RTrim()` 函数来完成：

```mysql
SELECT Concat(RTrim(vend_name), ' (', RTrim(vend_country), ')')
FROM Vendors
ORDER BY vend_name;
```

### 使用别名

别名是一个字段或值的替换名。

```mysql
SELECT Concat(RTrim(vend_name), ' (', RTrim(vend_country), ')') AS vend_title
FROM Vendors
ORDER BY vend_name;
```

用 `AS` 关键字赋予别名。

### 执行数学计算

```mysql
SELECT prod_id, quantity, item_price
FROM OrderItems
WHERE order_num = 20008;

SELECT prod_id,
quantity,
item_price,
quantity*item_price AS expanded_price
FROM OrderItems
WHERE order_num = 20008;
```

支持 `+`、`-`、`*` 和 `/` 四则运算。