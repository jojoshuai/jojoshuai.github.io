---
title: linux-进程线程协程
date: 2024-09-13 11:48:23
categories:
  - linux
tags:
  - 进程
  - 线程
  - 协程
---

> https://juejin.cn/post/6911302335595544584

1. 进程

对应 pcb

上下文切换
cpu 会划分时间片给每个进程
每次切进程需要对应一个 pcb 的保存和导入
![进程上下文切换图](进程上下文切换图.png)

2. 线程
