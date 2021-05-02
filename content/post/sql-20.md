---
title: "Sql 20：管理事务处理"
date: 2021-05-03T07:31:02+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## 事务处理

事务处理通过确保成批的 SQL 操作完全执行或完全不执行来维护数据库完整性。

<!--more-->

- 事务（transaction）指一组 SQL 语句；
- 回退（rollback）指撤销指定 SQL 语句的过程；
- 提交（commit）指将未存储的 SQL 语句结果写入数据库表；
- 保留点（savepoint）指事务处理中设置的临时占位符（placeholder），可以对它发布回退（与回退整个事务处理不同）。

```mysql
START TRANSACTION
...
```

使用 `START TRANSACTION` 控制事务处理。

```mysql
DELETE FROM Orders;
ROLLBACK;
```

使用 `ROLLBACK` 回退 SQL 语句。

```mysql
START TRANSACTION
DELETE OrderItems WHERE order_num = 12345
DELETE Orders WHERE order_num = 12345
COMMIT TRANSACTION
```

使用 `COMMIT` 显式提交 SQL 语句。

```mysql
SAVEPOINT delete1;

ROLLBACK TO delete1;
```

使用 `SAVEPOINT` 在事务处理块中创建保留点占位符来支持部分事务的回退。然后用 `ROLLBACK TO` 回退。

保留点越多越好。