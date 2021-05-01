---
title: "SQL 16：更新和删除数据"
date: 2021-05-01T23:30:17+08:00
categories: ["SQL"] 
tags: ["MySQL"]
draft: false
---

## 更新数据

```mysql
UPDATE Customers
SET cust_email = 'kim@thetoystore.com'
WHERE cust_id = 1000000005;
```

使用 `UPDATE` 语句更新数据，可以更新表中指定行，或者全部行。

## 删除数据

```mysql
DELETE FROM Customers
WHERE cust_id = 1000000006;
```

使用 `DELETE` 语句删除数据，可以删除表中的指定行，或者全部行。

## 指导原则

使用 `UPDATE` 或 `DELETE` 时所遵循的重要原则。

- 除非确实打算更新和删除每一行，否则绝对不要使用不带 `WHERE` 子句的 `UPDATE` 或 `DELETE` 语句。
- 保证每个表都有主键，尽可能像 `WHERE` 子句那样使用它（可以指定各主键、多个值或值的范围）。
- 在 `UPDATE` 或 `DELETE` 语句使用 `WHERE` 子句前，应该先用 `SELECT` 进行测试，保证它过滤的是正确的记录，以防编写的 `WHERE` 子句不正确。
- 使用强制实施引用完整性的数据库，这样 DBMS 将不允许删除其数据与其他表相关联的行。
- 有的 DBMS 允许数据库管理员施加约束，防止执行不带 `WHERE` 子句的 `UPDATE` 或 `DELETE` 语句。如果所采用的 DBMS 支持这个特性，应该使用它。