---
title: "软件测试 01：基础知识"
date: 2021-04-28T11:34:52+08:00
categories: ["Testing"]
tags: ["Software testing"]
draft: false
---

## 什么是 Bug

软件出现问题时，我们认为软件中有 Bug。为了更精确地描述什么是 Bug，先定义产品说明书（product specification）：软件开发小组的一个协定，对开发的产品进行定义，给出产品的细节、如何做、做什么、不能做什么。

软件 Bug 出现的判断规则：

1. 软件未实现产品说明书要求的功能；
2. 软件出现了产品说明书指明不应该出现的错误；
3. 软件实现了产品说明书未提到的功能；
4. 软件未实现产品说明书没明确提及但应该实现的目标；
5. 软件难以理解、不易使用、运行缓慢或用户会认为不好。

Bug 的首要来源是产品说明书，其次是设计。

将软件初期版本分发给小部分用户使用，称为 Beta 测试。

软件测试的目标是发现软件 Bug，且应该尽可能早地找出 Bug 并修复。

## 软件开发过程

软件需要的投入：

1. 客户需求；
2. 产品说明书；
3. 进度表；
4. 软件设计文档；
5. 测试文档。

软件除了代码以为，还包括：帮助文件、用户手册、样本和示例、标签和不干胶、产品支持信息、图标和标志、错误提示信息、广告和宣传材料、安装、说明文件等。

## 软件测试的实质

完全测试软件是不可能的，因为：

- 输入量太大；
- 输出结果太多；
- 软件执行路径太多；
- 软件说明书是主观的。

因为不能完全测试软件，所以软件测试是有风险的。需要学会如何吧数量巨大的可能测试减少到可控制的范围内，以及如何针对风险做出明智的抉择，哪些测试重要，哪些不重要。

测试无法显示潜伏的 Bug。

找到的 Bug 越多，说明软件 Bug 越多。

必须不断编写不同的、新的测试程序，对软件的不同部分进行测试，以找出更多 Bug。