---
title: "计算机网络 Chap1 03：影响网络性能的因素"
date: 2021-04-09T09:34:00+08:00
categories: ["Computer Networking"]
tags: ["计算机网络：自顶向下方法"]
draft: false
---

影响网络性能的三大要素：

1. 时延；
2. 丢包；
3. 吞吐量。

## 时延

分组在结点之间转移的过程中存在多种时延，其中最重要的几种：

- 结点处理时延（nodal processing delay）：检查分组首部和决定分组导向所需时间
- 队列时延（queuing delay）：等待传输所需时间
- 传输时延（transmission delay）：传输分组所有比特到链路中所需时间 $L/R$
- 传播时延（propagation delay）：从链路起点到路由器所需时间 $d/s$

四种时延累加起来称为结点总时延（total nodal delay）。
$$
d_\text{nodal}=d_\text{proc}+d_\text{queue}+d_\text{trans}+d_\text{prop}
$$
其中最关注队列时延 $d_\text{queue}$。

## 丢包

$a$ 为分组到达队列的平均速率，$R$ 为传输速率，每个分组设为 $L$ 比特。则到达队列的比特的平均速率为 $La$ 比特每秒，将 $La/R$ 称为流量强度（traffic intensity）。

$La/R>1$ 则比特到达队列的速率超过了比特从队列中传输出去的速率。

由于队列容量有限，分组到达时可能发现一个满的队列，此时路由器会丢弃该分组，即发生丢包。流量强度增加时，丢包的比例也会增加。

假设从源主机到目的地主机中有 $N-1$ 个路由器，则端到端时延
$$
d_\text{end-end}=N(d_\text{proc}+d_\text{trans}+d_\text{prop})
$$
Traceroute 程序用于测试网络时延。

## 吞吐量

瞬时吞吐量（instantaneous throughput）是任何时间瞬间主机接收文件的速率，单位为 bps。

平均吞吐量（average throughput）是主机接收文件所有比特的速率 $F/T$。

瓶颈链路（bottleneck link）指传输速率最小的链路。