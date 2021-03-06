---
title: "计算机网络 Chap1 04：互联网协议"
date: 2021-04-10T09:05:49+08:00
categories: ["Computer Networking"]
tags: ["计算机网络：自顶向下方法"]
draft: false
---

计算机网络架构是分层（layer）设计的。分层架构的好处是提供了一种模块化的方法，能对复杂系统的特定部分良定义，修改特定层提供的服务的实现变得简单，同时系统的剩余部分不会受到影响。

<!--more-->

每层为上层提供服务，称为层的服务模型（service model）。

每层提供服务的方式：

1. 执行该层的某些动作；
2. 直接使用来自下层的服务。

分层架构的缺点：

1. 每层可能提供与下层重复的功能；
2. 某层的功能可能需要仅在其他层出现的信息。

每个层的协议合起来称为协议栈（protocol stack）。互联网协议栈由五层组成，从下到上：

1. 物理层；
2. 链路层；
3. 网络层；
4. 传输层；
5. 应用层。

---

应用层位于协议栈的顶层，是网络应用和应用层协议所在的地方。应用层协议包括：

1. HTTP 协议；
2. SMTP 协议；
3. FTP 协议；
4. DNS 协议。

应用层的信息分组称为报文（message）。

---

传输层在应用端点之间传输应用层的报文。传输层协议：

1. TCP 协议；
2. UDP 协议。

传输层分组称为报文段（segment）。

---

网络层负责将主机的网络层分组移动到另一个主机。网络层协议：

1. IP 协议；
2. 多种路由协议。

网络层分组称为数据报（datagram）。

---

链路层负责在结点之间移动分组。

由链路层提供的服务取决于应用于该链路的特定链路层协议。

链路层分组称为帧（frame）。

---

物理层负责将帧中的每个比特从一个结点移动到下一个结点。

物理层的协议与具体链路、实际传输物理媒介有关。

---

封装（encapsulation）指某层将上层的分组和该层的首部信息合在一起形成新的分组。

每层的分组具有两种类型的字段：首部字段和有效载荷字段（payload field）。有效载荷通常来自于上一层的分组。