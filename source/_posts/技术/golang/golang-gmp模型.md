---
title: golang-gmp模型
date: 2024-09-13 11:43:17
categories:
  - golang
tags:
  - gmp
---

> 前置：
>
> - linux-内核态和用户态.md
> - linux-进程线程协程.md
>   引用：
>
> 1. https://juejin.cn/post/7412893752090312714
> 2. https://juejin.cn/post/7265939798794764322

基本概念
Go 语言使用 GMP 模型来管理并发执行，GMP 模型由三个核心组件组成：G（Goroutine）、M（Machine）、P（Processor）。

G（Goroutine）：Goroutine 是 Go 语言中的协程，代表一个独立的执行单元。Goroutine 比线程更加轻量级，启动一个 Goroutine 的开销非常小。Goroutine 的调度由 Go 运行时在用户态进行。
M（Machine）：M 代表操作系统的线程，负责实际执行 Go 代码。一个 M 可以执行多个 Goroutine，但同一时间只能执行一个 Goroutine。M 与操作系统的线程直接对应，Go 运行时通过 M 来利用多核 CPU 的并行计算能力。
P（Processor）：P 代表执行上下文（Processor）。P 管理着可运行的 Goroutine 队列，并负责与 M 进行绑定。P 的数量决定了可以并行执行的 Goroutine 的数量。Go 运行时会根据系统的 CPU 核数设置 P 的数量。
