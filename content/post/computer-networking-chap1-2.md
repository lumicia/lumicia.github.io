---
title: "计算机网络 Chap1 02：互联网边缘和核心"
date: 2021-04-07T08:22:47+08:00
categories: ["Computer Networking"]
tags: ["计算机网络：自顶向下方法"]
draft: false
---

## 互联网边缘：端系统

主机可进一步分为客户端（client）和服务器（server）。服务器大都位于大型的数据中心（data center）中。

<!--more-->

数据中心的三大目的：

1. 提供公司自身的商业服务；
2. 提供并行计算基础设施；
3. 提供云计算（cloud computing）服务给其他公司。

接入网（access network）是端系统与其他远程端系统连接的路径中，在物理上与第一个路由器连接的网络。这个路由器称为边缘路由器（edge router）。

住宅宽带接入的几种类型：

1. 数字用户线 DSL（digital subscriber line）；
2. 电缆互联网接入（cable Internet access），该系统使用了光纤和同轴电缆，因此也称为混合光纤同轴 HFC（hybrid fiber coax）；
3. 光纤入户 FTTH（fiber to the home），分为主动光纤网络 AON（active optical network）和被动光纤网络 PON（passive optical network）；
4. 5G 固定无线网（5G fixed wireless）。

企业、大学和家庭等通常使用局域网 LAN（local area network）将端系统连接到边缘路由器。LAN 的类型：

1. 以太网（Ethernet）
2. 局域无线网（Wifi）

广域无线网：3G、LTE 4G 和 5G。

物理媒介：

1. 导引型媒介（guided media）：光纤、双绞铜线、同轴电缆；
2. 非导引型媒介（unguided media）：陆地无线电通道、卫星无线电通道。

## 互联网核心：端系统的分组交换机和链路构成的网状结构

网络应用中的选系统彼此交换报文（message）。

长报文通常会划分为更小的数据块，称为分组（packet）。

每个分组通过通信链路和分组交换机从源端系统传输到目的端系统。

分组交换机分为路由器和链路层交换机。

通信链路以其最大传输速率传输分组，因此传输速率为 $R$ 比特每秒的链路传输 $L$ 比特的分组需要的时间为 $L/R$ 秒。

分组交换机在链路输入端使用存储转发传输（store-and-forward transmission）机制：分组交换机必须接收到一个完整的分组后才能将该分组传输到链路中。如果有 $N$ 条链路，速率均为 $R$，则端到端的时延为
$$
d_\text{end-to-end}=N\frac{L}{R}.
$$

每个分组交换机有多条链路相连接，每条链路对应一个**输出缓冲**（output buffer，或称为输出队列 output queue），存储路由器将要发送给该链路的分组。输出缓冲是分组交换中最重要的部分。

除存储转发时延外，分组还存在队列时延（queue delay）。

如果缓冲完全充满，将发生丢包（packet loss），要么将到达的分组丢弃，要么将已在队列中的分组之一丢弃。

每个端系统都有一个 IP 地址。源端系统将目的地的 IP 地址包含在分组的首部，从而将分组发送到目的地。

路由器具有一个转发表（forwarding table），将目的地的地址（或地址的一部分）映射到其出链路。分组到达路由器后，路由器检查地址并搜索转发表，找出适合该地址的出链路。

在链路和交换机的网络中移动数据的两种基本方法：

1. 电路交换（circuit switching）
2. 分组交换（packet switching）

在电路交换网络中，两个端系统沿路径通信所需要的资源在通信会话期间预留。

在分组交换网络中，这些资源不会预留，会话的报文按需使用资源，因此可能需要在队列中等待接入通信链路。

链路中的电路要么通过频分复用 FDM（frequency-division multiplexing）实现，要么通过时分复用 TDM（time-division multiplexing）实现。

频带的宽度称为带宽（bandwidth）。

分组交换的性能优于电路交换。

端系统通过接入 ISP 连接到互联网，接入 ISP 之间也必须相互连接。通过创建网络的网络来连接接入 ISP。

网络结构的可能构造演化：

1. 单一全球承载 ISP 互联所有接入 ISP；
2. 多个全球承载 ISP 连接大量接入 ISP；
3. 一级 ISP 连接区域 ISP，区域 ISP 连接接入 ISP；
4. 在结构 3 的基础上加入存在点 PoP（point of presence）、多宿（multi-home）、对等（peer）和互联网交换点 IXP（Internet exchange point）；
5. 在结构 4 的基础上加入内容提供商网络（content-provider network），形成如今的互联网。