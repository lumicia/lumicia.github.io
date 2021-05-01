---
title: "SQL 17：创建和操纵表"
date: 2021-05-02T07:06:33+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## 创建表

创建表的两种方式：

- DBMS 自带的 GUI 工具；
- SQL 语句。

<!--more-->

用 `CREATE TABLE` 语句编程式地创建表。

```mysql
CREATE TABLE Products
(
    prod_id     CHAR(10)       NOT NULL,
    vend_id     CHAR(10)       NOT NULL,
    prod_name   CHAR(254)      NOT NULL,
    prod_price  DECIMAL(8,2)   NOT NULL,
    prod_desc   VARCHAR(1000)  NULL
);
```

允许 `NULL` 值的列也允许在插入行时不给出该列的值。不允许 `NULL` 值的列不接受没有列值的行，换句话说，在插入或更新行时，该列必须有值。每个表列要么是 `NULL` 列，要么是 `NOT NULL` 列，这种状态在创建时由表的定义规定。

只有不允许 `NULL` 值的列可作为主键，允许 `NULL` 值的列不能作为唯一标识。

```mysql
CREATE TABLE OrderItems
(
    order_num    INTEGER        NOT NULL,
    order_item   INTEGER        NOT NULL,
    prod_id      CHAR(10)       NOT NULL,
    quantity     INTEGER        NOT NULL  DEFAULT 1,
    item_price   DECIMAL(8,2)   NOT NULL
);
```

使用 `DEFAULT` 关键字在列定义中指定默认值。

## 更新表

使用 `ALTER TABLE` 语句更新表。

```mysql
ALTER TABLE Vendors
ADD vend_phone CHAR(20);
```

必须指定数据类型。

## 删除表

使用 `DROP TABLE` 语句删除表。

```mysql
DROP TABLE CustCopy;
```

## 重命名表

MySQL 使用 `RENAME` 语句重命名表。