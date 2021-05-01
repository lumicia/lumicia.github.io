---
title: "SQL 15：插入数据"
date: 2021-05-01T23:14:35+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## 插入数据

使用插入的方式：

- 插入单个完整行；
- 插入单个部分行；
- 插入查询的结果。

```mysql
INSERT INTO Customers
VALUES(1000000006,
       'Toy Land',
       '123 Any Street',
       'New York',
       'NY',
       '11111',
       'USA',
       NULL,
       NULL);
```

使用 `INSERT` 插入数据。插入完整行。

```mysql
INSERT INTO Customers(cust_id,
                      cust_name,
                      cust_address,
                      cust_city,
                      cust_state,
                      cust_zip,
                      cust_country)
VALUES(1000000006,
       'Toy Land',
       '123 Any Street',
       'New York',
       'NY',
       '11111',
       'USA');
```

插入部分行。

```mysql
INSERT INTO Customers(cust_id,
                      cust_contact,
                      cust_email,
                      cust_name,
                      cust_address,
                      cust_city,
                      cust_state,
                      cust_zip,
                      cust_country)
SELECT cust_id,
       cust_contact,
       cust_email,
       cust_name,
       cust_address,
       cust_city,
       cust_state,
       cust_zip,
       cust_country
FROM CustNew;
```

插入检索数据。

## 复制一个表到另一个表

```mysql
CREATE TABLE CustCopy AS SELECT * FROM Customers;
```

`CREATE TABLE` 复制数据到一个新的表。