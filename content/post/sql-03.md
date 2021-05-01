---
title: "SQL 03：排序检索数据"
date: 2021-04-27T16:15:43+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## `ORDER BY` 子句

```mysql
SELECT prod_name
FROM Products
ORDER BY prod_name;
```

使用 `ORDER BY` 子句排序检索到的数据。

<!--more-->

注意：

- `ORDER BY` 必须是 `SELECT` 语句的最后一个子句。

### 按多列排序

```mysql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price, prod_name;
```

用 `,` 分隔多列，按列名顺序排序。

### 按列位置排序

```mysql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY 2, 3;
```

使用列的位置来替代列名，可以减少打字的数量。

缺点：

1. 可能指定错误的列。
2. 改变了 `SELECT` 列表后，没有及时更新列的位置从而导致错误排序。
3. 不能排序不在 `SELECT` 列表中的列。

### 指定排序方向

```mysql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC;
```

排序默认升序，可以用 `DESC` 关键字改成降序。

```mysql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC, prod_name;
```

`DESC` 只应用于它之前的列名。如果想在多个列上进行降序排序，必须对每一列指定 `DESC` 关键字。

`DESC` 是 `DESCENDING` 的缩写。