---
title: "计算机网络 Chap2 06：DNS"
date: 2021-04-21T08:29:48+08:00
categories: ["Computer Networking"]
tags: ["计算机网络：自顶向下方法"]
draft: false
---

互联网上有非常多的主机，通过主机名（hostname）来标识主机。主机名的优点是容易记忆。主机名的缺点：

- 缺少关于主机位置的信息；
- 是由字母和数字组成的可变长度的字符串，给路由器处理增加了难度。

为了克服主机名的缺点，出现了 IP 地址。IP 地址由四个字节组成，具有严格的层级结构。

<!--more-->

主机名和 IP 地址之间的转换则是通过域名系统 DNS（domain name system）实现。

DNS 是：

- 在分层的 DNS 服务器中实现的一个分布式数据库；
- 允许主机查询分布式数据库的应用层协议。

DNS 服务器通常是运行 Berkeley Internet Name Domain（BIND）软件的 UNIX 机器。

DNS 提供的其他服务：

- 主机别名；
- 邮件服务器别名；
- 负载分配。

## DNS 工作原理

应用调用 DNS 的客户端一侧，指定需要转换的主机名。然后用户主机中的 DNS 得到控制权，向网络中发送查询信息。所有 DNS 查询和回答报文在 UDP 数据报中发送到 53 端口。经过一定延迟后，用户主机中的 DNS 接收一个 DNS 回答报文，提供所期望的映射。映射之后传递给调用程序。

DNS 服务器采用分布式和层级设计：

- 根 DNS 服务器；
- 顶级域 DNS 服务器；
- 权威 DNS 服务器。

本地 DNS 服务器不属于层级结构，但非常重要。每个 ISP 都有一个本地 DNS 服务器。

主机发送查询报文给本地 DNS 服务器，本地 DNS 服务器依次将查询报文转发给根 DNS 服务器、对应后缀的顶级域 DNS 服务器和权威 DNS 服务器。总共发送八个 DNS 报文：四个查询报文和四个回答报文。

任何 DNS 查询都可能是递归的或迭代的。

DNS 缓存用于提升延迟性能，减少 DNS 报文在互联网上到处传输的数量。

## DNS 记录和报文

共同实现了 DNS 分布式数据库的  DNS 服务器存储资源记录 RR（resource record）。RR 提供了主机名到 IP 地址的映射。

一个 RR 是一个四元组：`(Name, Value, Type, TTL)`。TTL 指 RR 存在的时间。Type 的值决定 Name 和 Value 的值：

- `Type=A`，`Name` 是主机名，`Value` 是主机名对应的 IP 地址；
- `Type=NS`，`Name` 是域名，`Value` 是权威 DNS 服务器的主机名，知道如何获得该域名中主机的 IP 地址；
- `Type=CNAME`，`Name` 是别名，`Value` 是别名的规范主机名；
- `Type=MX`，`Name` 是别名，`Value` 是邮件服务器的规范主机名。

DNS 仅有查询和回答两种报文，且格式相同。

- 前 12 个字节是首部区域。第一个字段是一个 16 比特的数字，用于识别查询。还有一些标志字段。
- 问题区域，包含正在进行的查询的信息。
- 回答区域，包含一开始查询的名字的资源记录。
- 权威区域，包含其他权威服务器的记录。
- 附加区域，包含有帮助的记录。