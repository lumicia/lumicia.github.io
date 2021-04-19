---
title: "计算机网络 Chap2 05：HTTP/2"
date: 2021-04-19T10:51:37+08:00
categories: ["Computer Networking"]
tags: ["计算机网络：自顶向下方法"]
draft: false
---

HTTP/2 在 2015 年成为标准。HTTP/2 的首要目标是通过让请求和响应在单个 TCP 连接上多路复用来减少感知延迟，提供请求优先级和服务器推送，提供高效压缩的 HTTP 首部字段。HTTP/2 改变了客户端和服务器之间数据如何格式化和传输的方式。

<!--more-->

通过每个网页的单 TCP 连接，减少了服务器的套接字数量，并且每个传输的网页公平共享网络带宽。但也带来了 **HOL 阻塞**（Head of Line blocking）问题。

HOL 阻塞指网页靠近顶部的位置有一个大文件，然后有许多小文件在大文件后面。由于链路速率限制，单 TCP 连接可能导致该大文件花费较长时间来传输，与此同时其他小文件由于在大文件后面而延迟传输，从而导致大文件阻塞小文件的情况出现。

HTTP/1.1 通过打开多个并行的 TCP 连接来解决 HOL 阻塞问题。HTTP/2 通过避免并行的 TCP 连接传输单个网页，减少了服务器需要打开和维持的套接字数量，也让 TCP 拥塞控制如预期运行。

HTTP/2 解决 HOL 阻塞的方式是将每个报文分成小的帧（frame），并在同一 TCP 连接中交错发送（interleave）请求报文和响应报文。将大文件和小文件分别分成多个帧。每发送大文件的一个帧，就发送每一个小文件的一个帧。

帧分割通过 HTTP/2 协议的 framing sub-layer 实现。

帧到达客户端后，在 framing sub-layer 还原成原始的响应报文，然后像往常一样由浏览器处理。同理，客户端的请求报文也分割成帧，然后交错发送。

---

当客户端发送并发请求时，可以通过对每个报文赋予一个值在 1 到 256 之间的权重，来调整所请求的响应的优先级。值越大，优先级越高。另外客户端也可以通过指定所依赖报文的 ID，来说明每个报文对其他报文的依赖。

HTTP/2 的另一个特性是服务器可以为一个客户端请求发送多个响应，即服务器可以推送（push）额外的对象给客户端，而客户端不必一个一个地请求。服务器可以分析 HTML 页面，标识所需对象，然后在显式接收这些对象的请求前发送给客户端。服务器推送减少了等待请求导致的额外延迟。

---

HTTP/3 在 UDP 协议的基础上实现了 QUIC 协议。QUIC 协议特性包括：

- 报文多路复用（交错发送）；
- per-steam flow control；
- 低延迟连接建立。

HTTP/3 是被设计为在 QUIC 上运行的新 HTTP 协议。