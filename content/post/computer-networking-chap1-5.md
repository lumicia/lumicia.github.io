---
title: "计算机网络 Chap1 05：网络安全"
date: 2021-04-14T09:19:35+08:00
categories: ["Computer Networking"]
tags: ["计算机网络：自顶向下方法"]
draft: false
---

如今互联网极大地改变了人们的生活方式，给人们带来了许多便利。但如同硬币的两面，互联网也可能带来麻烦：

- 个人隐私可能会被窃取；
- 网络服务可能发生中断，导致生产生活瘫痪；

因此保证网络安全是十分重要的，网络安全已经是互联网发展的一个核心领域。

网络攻击的几种方式：

1. 恶意软件（malware）通过互联网入侵计算机，窃取私人信息、删除或锁定私人文件，或将计算机变成僵尸网络（botnet）的一部分等。恶意软件通常会自我复制（self-replicating）。
2. 对服务器和网络基础设施发动拒接服务攻击 DoS（denial-of-service）。Dos 攻击主要分为三种：弱点攻击（vulnerability attack）、带宽洪泛（bandwidth flooding）、连接洪泛（connection flooding）。带宽洪泛常使用 DDoS（distributed DoS）攻击。
3. 通过互联网嗅探分组（sniff packet），从而窃取传输的分组。嗅探分组的接收机称为分组嗅探机（packet sniffer）。分组嗅探机是被动的，不会主动将分组注入到通道中，因此难以检测。防御方法涉及到密码学。
4. 伪装成受信任的人。使用 IP 哄骗（IP spoofing）将具有虚假地址的分组注入互联网，来冒充一个用户。可以通过端点鉴别（end-point authentication）来解决。