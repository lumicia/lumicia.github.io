---
title: "计算机网络 Chap2 02：HTTP 协议"
date: 2021-04-16T11:33:18+08:00
categories: ["Computer Networking"]
tags: ["计算机网络：自顶向下方法"]
draft: true
---

HTTP 协议（HyperText Transfer Protocol）是 Web 的应用层协议。

HTTP 分别在客户端程序和服务器程序中实现。两个程序在不同的端系统中运行，通过交换 HTTP 报文来通信。HTTP 定义了报文的结构和通信方式。

<!--more-->

网页（Web page）通常由一个基本 HTML 文件和其他引用对象组成。

Web 浏览器（browser）实现了客户端的 HTTP 协议，因此等价于 Web 客户端。

Web 服务器实现了服务器端的 HTTP 协议。

HTTP 也定义了 Web 客户端如何从 Web 服务器请求网页，及服务器如何将网页传输给客户端。

HTTP 使用 TCP 作为默认的传输层协议。

HTTP 协议是无状态协议（stateless protocol）。服务器不会存储关于客户端的状态信息。

在客户端和服务器通信期间，如果每个请求 / 响应对通过单独的 TCP 连接发送，称为非持续性连接（non-persistent connection）；通过同一个 TCP 连接发送，则称为持续性连接（persistent connection）。这两种连接方式 HTTP 都能使用，默认使用持续性连接。