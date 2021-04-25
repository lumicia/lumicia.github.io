---
title: "计算机网络 Chap3 01：传输层"
date: 2021-04-24T09:31:32+08:00
categories: ["Computer Networking"]
tags: ["计算机网络：自顶向下方法"]
draft: false
---

传输层协议为不同主机上的应用进程之间提供了逻辑通信（logical communication），两个主机就像是直接连接在了一起一样。

传输层协议在端系统中实现，而网络路由器中则未实现。

在发送端，传输层从应用进程接收到应用层报文，然后将其分成较小的块（chunk），每个块添加传输层首部，转换成为传输层分组，称为传输层报文段（segment）。

传输层接着将报文段传输到网络层，报文段封装为网络层分组，称为数据报（datagram）。注意网络路由器只会处理数据报中的网络层字段。

在接收端，网络层从数据报中提取传输层报文段，并传递给传输层。接着传输层处理报文段。

传输层和网络层的一个微妙区别：传输层在不同主机的进程之间提供逻辑通信，网络层在主机之间提供逻辑通信。

网络层协议称为 IP 协议（Internet Protocol）。IP 服务模型是尽力而为交付服务（best-effort delivery service），IP 会尽力在通信主机之间交付报文段，但不会保证。因此 IP 是一种不可靠服务（unreliable service）。每个主机都有一个 IP 地址。

UDP 和 TCP 最基础的责任是将两个端系统中的 IP 的交付服务扩展为两个在端系统中运行的进程之间的交付服务。这种扩展称为传输层多路复用（transport-layer multiplexing）和多路分解（demultiplexing）。

UDP 和 TCP 也通过在报文段首部包含错误检测字段来提供完整性检查。

TCP 另外提供了可靠数据传输（reliable data transfer）、拥塞控制（congestion control）。