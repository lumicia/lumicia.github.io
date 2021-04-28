---
title: "Sql 06：用通配符过滤"
date: 2021-04-28T09:50:08+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

## LIKE 谓词

在 `LIKE` 谓词中使用通配符，过滤符合特定搜索模式的数据。

通配符只能用于文本字段。

### % 通配符

```mysql
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE 'Fish%';
```

`%` 表示匹配任何字符出现任意次数。`Fish%` 匹配任意以 `Fish` 开头的单词。`%bean bag%` 匹配任意包含 `bean bag` 文本的值。

`%` 也会匹配零个字符。

注意结尾空格。

### _ 通配符

```mysql
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE '__ inch teddy bear';
```

`_` 匹配单个字符。

同样要注意结尾空格。

### [] 通配符

```mysql
SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[JM]%'
ORDER BY cust_contact;
```

`[]` 通配符用于指定一个字符的集合，必须匹配指定位置的一个字符。

MySQL 不支持字符集合。

### 通配符使用注意事项

1、不要过渡使用通配符，避免带来性能问题。

2、尽量不要在搜索模式的开头使用通配符。

3、注意通配符的位置。