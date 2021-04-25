---
title: "计算机网络 Chap1 01：互联网"
date: 2021-04-04T21:10:12+08:00
categories: ["Computer Networking"]
tags: ["计算机网络：自顶向下方法"]
draft: false
---

连接到互联网的设备称为主机（host）或端系统（end system）。端系统之间通过通信链路（communication link）和分组交换机（packet switch）的网络相连接。

<!--more-->

链路的传输速率（transmission rate）用每秒传输的比特度量，单位 bit/s 或 bps。

分组（packet）指发送端系统将数据分段，给每个分段添加头部字节后形成的信息包。分组通过网络传输到目标端系统，然后重组为原来的数据。

分组交换机主要有路由器（router）和链路层交换机（link-layer switch）。

一个分组从发送端系统到接收端系统经过的通信链路和分组交换机称为经过网络的路由（route）或路径（path）。

端系统通过互联网服务提供商 ISP（Internet service provider）接入互联网。

端系统、分组交换机等通过运行协议（protocol）来控制互联网中信息的发送和接收。

最重要的两个协议 TCP（Transmission Control Protocol）和 IP（Internet Protocol）协议。

从另一个角度看，互联网就是为应用提供服务的基础设施。互联网应用运行在端系统之上。

套接字接口（socket interface）规定了运行于端系统上的程序如何请求互联网基础设施将数据传输到另一个端系统上的特定目标程序。

为了进一步理解互联网，需要了解以下问题：

- 什么是分组交换机和 TCP/IP？
- 什么是路由器？
- 互联网使用哪种通信链路？
- 什么是分布式应用？
- 恒温器和体重秤是如何连接到互联网的？

**协议定义了两个通信实体之间消息交换的格式和顺序，以及传输和/或接收消息或其他事件时的动作**。