---
title: "计算机网络 Chap3 03：UDP"
date: 2021-05-03T22:11:17+08:00
categories: ["Computer Networking"]
tags: ["计算机网络：自顶向下方法"]
draft: false
---

UDP 是一种仅具有基本多路复用 / 多路分解功能和轻量差错检查的传输协议。

<!--more-->

UDP 工作流程：

- UDP 从应用进程得到报文，然后将源端口号和目的端口号字段附加到报文上来实现多路复用 / 多路分解服务，再添加两个其他小的字段，最后将得到的报文段传递给网络层。
- 网络层将传输层报文段封装为 IP 数据报，并尽力将其交付给接收主机。
- 如果网络层报文段到达接收主机，UDP 使用目的端口号将报文段数据交付给正确的应用进程。

值得注意的是 UDP 中的发送和接受传输层实体在发送报文段之前没有握手阶段，因此 UDP 是无连接的。

DNS 使用 UDP 协议。

UDP 相比 TCP 的优点：

- 在应用级别能更好地控制发送什么数据，什么时候发送。
- 没有建立连接。
- 没有连接状态。
- 较小的分组首部开销。

如果在应用层实现了可靠性，那么应用使用 UDP 也可以实现可靠数据传输。

## UDP 报文段结构

<img src="https://i.loli.net/2021/05/04/tGyhFWMupiHc7NI.png" alt="UDP segment structure" style="zoom:33%;" />

UDP 报文结构由首部和数据字段组成。首部有四个字段，每个字段大小为 2 字节：

- 源端口号；
- 目的端口号；
- 长度端口号：指定 UDP 报文段中字节的数量（首部加数据字段）；
- 检验和：接收主机用来检验报文段中是否存在差错；
- 数据字段：应用数据占据数据字段。

## UDP 检验和

UDP 检验和（checksum）提供了差错检测，查明 UDP 报文段中的比特从源移动到目的地是否改变了。

UDP 在发送端对报文段中所有 16 比特的词之和执行 1 的补码，得到检验和。在接收端将所有四个 16 比特的词相加，如果分组中没有差错发生，则得到的和所有位都是 1；如果某个比特是 0，则可以确定分组中发生差错。

虽然链路层协议也有差错检测，但不能保证源到目的地之间所有链路都提供了差错检测。另外差错也可能发生在路由器内存中。因此 UDP 必须在传输层提供差错检测，才能在端到端的基础上为端到端数据传输服务提供差错检测。

端到端原则（end-end principle）：因为某种功能必须基于端到端实现，与将功能放在较高级别的代价相比，放在较低级别的功能可能是多余或没有价值的。

虽然 UDP 提供差错检测，但对于差错恢复无能为力。