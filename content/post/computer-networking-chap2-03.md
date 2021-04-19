---
title: "计算机网络 Chap2 03：HTTP 报文格式"
date: 2021-04-19T08:54:35+08:00
categories: ["Computer Networking"]
tags: ["计算机网络：自顶向下方法"]
draft: false
---

HTTP 报文分为 HTTP 请求报文和 HTTP 响应报文。报文都是 ASCII 文本。报文每行结尾由一个回车符（carriage return）和一个换行符（line feed）组成。报文最后一行结尾还需额外附加一行的回车符和换行符。

<!--more-->

## HTTP 请求报文

HTTP 请求报文的第一行称为请求行（request line）。接下来的行称为首部行（header line）。

请求行有三个字段：

1. 方法字段；
2. URL 字段；
3. HTTP 版本字段。

方法字段包括：`GET`、`POST`、`HEAD`、`PUT`、`DELETE` 等。

`GET` 方法使用最多。`GET` 方法用于浏览器请求一个对象，该对象在 URL 字段中标识。

在首部行后面是实体体（entity body）。使用 `GET` 方法时实体体为空。用 `POST` 方法时可以使用实体体。HTTP 客户端通常在用户提交表单时使用 `POST` 方法，实体体会包含用户输入到表单字段中的内容。

提交表单也可以用 `GET` 方法。这将在所请求的 URL 中包含输入到表单字段中的数据。

`HEAD` 方法类似于 `GET` 方法，应用开发者通常用来进行调试。

`PUT` 方法通常用于和 Web 发布工具一起使用。`PUT` 方法也在应用中用于上传对象到 Web 服务器中。

`DELETE` 方法用于删除 Web 服务器中的对象。

首部行例子：`Host`、`Connection`、`User-agent`、`Accept-language`。

## HTTP 响应报文

响应报文分为三部分：

- 初始状态行（initial status line）；
- 首部行；
- 实体体。

实体体包含被请求对象自身。

状态行的三个字段：

- 协议版本字段；
- 状态码；
- 对应状态消息。

首部行例子：`Connection`、`Date`、`Server`、`Last-Modified`、`Content-Length`、`Content-type`。

状态码例子：

- `200 OK`：请求成功，在响应中返回信息；
- `301 Moved Permanently`：请求对象已永久移动，在 `Location` 中指定新 URL，客户端会自动获取新 URL；
- `400 Bad Request`：通用错误码，服务器无法理解请求；
- `404 Not Found`：服务器不存在所请求的文档；
- `505 HTTP Version Not Supported`：服务器不支持请求 HTTP 协议版本。