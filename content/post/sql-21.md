---
title: "SQL 21：使用游标"
date: 2021-05-03T07:53:15+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## 游标

一个 SQL 查询检索得到的结果称为结果集。结果集中返回的行都是与SQL语句相匹配的行（零行或多行）。

<!--more-->

有时，需要在检索出来的行中前进或后退一行或多行，这就是游标的用途所在。游标（cursor）是一个存储在 DBMS 服务器上的数据库查询，它不是一条 `SELECT` 语句，而是被该语句检索出来的结果集。在存储了游标之后，应用程序可以根据需要滚动或浏览其中的数据。

使用游标涉及几个明确的步骤：

- 在使用游标前，必须声明（定义）它。这个过程实际上没有检索数据，它只是定义要使用的SELECT语句和游标选项。
- 一旦声明，就必须打开游标以供使用。这个过程用前面定义的SELECT语句把数据实际检索出来。
- 对于填有数据的游标，根据需要取出（检索）各行。
- 在结束游标使用时，必须关闭游标，可能的话，释放游标（有赖于具体的DBMS）。

声明游标后，可根据需要频繁地打开和关闭游标。在游标打开时，可根据需要频繁地执行取操作。

```mysql
DECLARE CustCursor CURSOR
FOR
SELECT * FROM Customers
WHERE cust_email IS NULL;
```

使用 `DECLARE` 语句声明游标。

```mysql
OPEN CURSOR CustCursor
```

使用 `OPEN CURSOR` 来打开游标。

```mysql
CLOSE CustCursor
```

使用 `CLOSE` 关闭游标。