---
title: "SQL 学习 01：学习准备"
date: 2021-04-15T14:36:38+08:00
categories: ["SQL"]
tags: ["MySQL"]
draft: false
---

学习前需要安装一个数据库，我选择了 MySQL。Windows 下载链接：[MySQL Installer for Windows](https://dev.mysql.com/downloads/windows/)。注意安装 server 时可能需要安装 Visual C++ 运行库。

然后下载 《SQL 必知必会》一书用于 MySQL 的样表。下载链接：[Sample Tables](https://forta.com/wp-content/uploads/books/0135182794/TYSQL5_MySQL.zip)。其他数据库的样表可在 http://forta.com/books/0135182794/ 找到。

<!--more-->

将 MySQL 的可执行文件安装路径 `C:\Program Files\MySQL\MySQL Server 8.0\bin` 加入到环境变量中。

创建用户：

```bash
create user 'username'@'%' identified by 'userpassword';
```

授权：

```bash
grant all privileges on *.* to 'username'@'%' with grant option;
```

登录 MySQL：

```bash
mysql -u username -p
```

按下回车，然后输入密码。

修改密码：

```bash
ALTER USER 'username'@'localhost' IDENTIFIED WITH MYSQL_NATIVE_PASSWORD BY 'newpassword';
```

（可能会用到就先记下来）修改加密规则：

```bash
ALTER USER 'username'@'%' IDENTIFIED BY 'userpassword' PASSWORD EXPIRE NEVER; 
```

创建一个新数据库 `tysql`：

```bash
create database tysql charset=utf-8;
```

查看并使用数据库：

```bash
show databases;
use tysql;
```

将样表 `create.txt` 和 `populate.txt` 中的内容复制粘贴到命令行，创建新表：

```bash
// create.txt
CREATE TABLE Customers
(
  cust_id      char(10)  NOT NULL ,
  cust_name    char(50)  NOT NULL ,
...

// populate.txt
INSERT INTO Customers(cust_id, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country, cust_contact, cust_email)
...
```

查看当前数据库中的表：

```bash
show tables;
```

