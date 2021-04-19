---
title: "计算机网络 Chap2 04：cookie 和 Web 缓存"
date: 2021-04-19T10:11:44+08:00
categories: ["Computer Networking"]
tags: ["计算机网络：自顶向下方法"]
draft: true
---

## Cookie

HTTP 服务器是无服务的。但通常会希望网站能识别用户，不管是因此服务器希望限制用户访问，或因为希望将服务内容作为一种用户识别的功能。cookie 可以实现以上目的，网站通过 cookie 保持跟踪用户。

<!--more-->

cookie 技术包括四个部分：

1. 位于 HTTP 响应报文的 cookie 首部行；
2. 位于 HTTP 请求报文的 cookie 首部行；
3. 保存在用户端系统的 cookie 文件，通过用户的浏览器管理；
4. 网站后端数据库。

## Web 缓存

Web 缓存（Web cache）也称为代理服务器（proxy server），是一种网络实体，代表原始 Web 服务器来满足 HTTP 请求。

Web 缓存具有自己的磁盘存储，并保存最近的请求对象的拷贝到存储中。

Web 缓存既是服务器也是客户端。

Web 缓存可以大量减少客户端的响应时间；也可以大量减少机构到互联网的接入链路的流量；还可以大量减少互联网上的整体 Web 流量，从而提升所有应用的性能。

Web 缓存使用内容分发网络 CDN（Content Distribution Network）技术。

Web 缓存带来一个新问题：缓存中存储的对象拷贝可能过时。HTTP 使用条件 GET（conditional GET）机制来验证缓存中的对象是最新的。

一个条件 GET 报文是一个 HTTP 请求报文，满足以下条件：

1. 请求报文使用 `GET` 方法；
2. 请求报文包含 `If-Modified-Since:` 首部行。

`If-Modified-Since` 首部行的值等于最近一次响应报文中 `Last-Modified:` 首部行的值。

如果请求对象没有修改，则服务器的响应报文中不包含请求对象，状态行的状态码为 `304 Not Modified`。