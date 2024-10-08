---
title: Prometheus
date: 2024-05-14 14:53:33
categories:
  - 计算机体系
tags:
  - prometheus
---

# Prometheus 简介

prometheus 是一个开源的指标监控引擎。其基本流程为：采集数据-存储数据-触发告警

### 1. Prometheus 的功能

#### 1.1 数据采集

Prometheus 同时支持基于 push 和 pull 的数据采集方式，但主要是 pull。push 需要配合 push gateway 来使用。

基于 pull 做指标采集的关键问题就是找到要 pull 的对象，即：服务发现。它需要能应付海量的、随时上下线、随时故障的监控目标：

1. 自动、实时的发现新的监控目标
2. 能够区分监控目标正常下线和无法访问的两种状态
3. 根据数据内容和监控目标，过滤、删改原始数据
4. 根据产生数据的监控目标，自动为监控数据添加元数据

> 思考：如果需要实现以上的目标，则 prometheus 需要支持各种流行的资源/服务管理系统，如 k8s 等，结合他们的能力提供服务发现功能。用户需要定义发现监控目标的策略，以及如何从这些目标上采集数据、采集哪写数据，以及是否做一定的预处理。满足服务发现策略、但有无法访问的目标，会被识别、标记。
> 如果无法使用已支持服务发现的外部系统，也可以基于文件或 DNS 来发现监控目标。

#### 1.2 数据存储：时序数据库

Prometheus 使用其自带的时序数据库 TSDB，这个 TSDB 的数据压缩方式是基于 Facebook 的 Gorilla TSDB 来开发的，能够做到样本平均大小 1.3bytes 左右的压缩比，单实例支持每秒数百万样本的采集量

#### 1.3 告警引擎

Prometheus 支持报警规则解析和心跳式的报警上报；同时提供了专门的告警通知管理组件 altermanager，做告警的聚合、分发和抑制。

### 2. Prometheus 不能做什么

1. 存储非时序数据，如日志、事件、tracing。
2. 对精准度要求极高的场景，如金融级或者说设计钱的监控。Prometheus 专门为监控场景做了性能优化，通过牺牲 0.1%的精准度换取了极佳的性能。
3. 对时序数据库功能的丰富性有要求的场景：Prometheus 不支持修改已采集的数据，不支持批量导入非实时数据。

> Prometheus 不是很擅长处理大事件范围的数据，如 1 个月以上的。因为 Prometheus 并没有对这些场景做优化，原因是借鉴于 Gorilla 的经验

### 3. 架构

![Prometheus架构图](https://github.com/prometheus/prometheus/blob/main/documentation/images/architecture.svg "Prometheus架构图")

#### 3.1 数据采集：Client Library 和 Exporters

Prometheus 会从外部的一个 Service Discover 里寻找需要采集数据的 endpoint，这些 endpoint 会通过引入 Client Library 来提供一个 metrics Api。这里的 metrics Api 是我们需要观测的服务(数据源)暴露的，这些 api 必须满足 metrics 定义的格式。

这样的 api 分两类，一类是可以引入 client library 提供 Api 的服务，一类是需要使用 exporter 服务(即**探针**)。

##### Client Libray

- 直接提供 api 的服务一般使用 client library 来提供 api 服务，Prometheus 有[大部分流行语言的 client library](https://prometheus.io/docs/instrumenting/clientlibs/)。

client library 支持[两种格式](https://prometheus.io/docs/instrumenting/exposition_formats/)的 api：

Prometheus 原生格式，如下

```
http_requests_total{method="post",code="200"} 1027 1395066363000
http_requests_total{method="post",code="400"}    3 1395066363000
http_request_duration_seconds_bucket{le="0.05"} 24054
http_request_duration_seconds_bucket{le="1"} 133988
http_request_duration_seconds_bucket{le="+Inf"} 144320
rpc_duration_seconds{quantile="0.01"} 3102
rpc_duration_seconds_sum 1.7560473e+07
rpc_duration_seconds_count 2693
```

[OpenMetrics](https://github.com/OpenObservability/OpenMetrics/blob/main/specification/OpenMetrics.md)格式。OpenMetrics 支持开发者手动标明数据时间戳(虽然不推荐)。同时支持 exemplar，exemplar 是用于关联以 trace 为主的非时序数据。
如下

```
# TYPE foo histogram
foo_bucket{le="0.01"} 0
foo_bucket{le="0.1"} 8 # {} 0.054
foo_bucket{le="1"} 11 # {trace_id="KOO5S4vxi0o"} 0.67
foo_bucket{le="10"} 17 # {trace_id="oHg5SJYRHA0"} 9.8 1520879607.789
foo_bucket{le="+Inf"} 17
foo_count 17
foo_sum 324789.3
foo_created  1520430000.123
```

##### Exporter

- exporter 服务适合不提供第三方接口的服务，例如[虚拟机、数据库，存储，队列等](https://prometheus.io/docs/instrumenting/exporters/)。exporter 又叫探针或导出器，特只本身并不是监控对象、专门通过各种手段从不提供 metrics Api 的服务那里获取数据，然后转成 metrics Api 的格式暴露出来的服务。

额外的，还有 push gateway 的方式来 push 数据。

> 为什么 prometheus 选择 pull：
> pull 优势：
>
> 1. 方便采集监控目标的元信息：结合服务发现和控制面能知道更多详细信息
> 2. 方便判断监控目标的健康状态：不需要额外引入一套系统来追踪所有监控对象的健康状态
> 3. 自主决定采集数据的时机。一方面不需要考虑高并发等问题，一方面不需要考虑采集数据的数据到达先后，从而引发的时间戳问题
>    pull 劣势：
> 4. 短生命周期的监控目标，容易被遗漏

#### 3.3 存储：TSDB 或 外部存储

TSDB 即 Prometheus 存储数据的数据库。

- 它有一个明显的缺点：原生不支持任何形式的多活，无法实现不同实例间的数据同步。这意味着不借助外力，Prometheus 会必然存在性能瓶颈和单点故障。
- 一般通过如下几种思路解决：
  - 为 Prometheus 挂载一个网络存储，搭配类似 k8s 的服务调度系统，允许其在各节点间漂移。
  - 使用 Remote Read/Write 功能对接一个优化得当的外部时序数据库。
    - Remote Read/Write 是两个功能，Read 允许 Prometheus 去外部时序数据库读取数据，Write 可以让 Prometheus 给 TSDB 写的同时也写一份给外部数据库。
    - PS：给 TSDB 写一份的过程可以去掉，但 Prometheus 的开发者认为：如果把数据保留时间设的比较短，如 2 小时，则不会造成显著的浪费，这些热点数据反而可以加速查询，直接关闭的价值不高。
  - 联邦机制：仍然是使用 remote read 和 write，A B 只 read, C D 只写入，E F G 被 wirte 和 read，用 EFG 的多实例来处理性能和单点。

#### 3.3 数据查询展示

Prometheus 有一套自己的数据查询语言，PromQL(Prometheus Query Language)，所有的 query api 都基于 PromQL。
Prometheus 自带的前端在`http://localhost:9090`，用来执行 query，查看指标等。但一般社区中使用[Grafana](https://grafana.com/)来作为前端。

#### 3.4 告警引擎

Prometheus 告警的核心机制是 Alerting rule，由规则+持续时间组成，它是写在可热加载的配置文件里的告警规则。如果这条规则能查询到数据，则 pening，如果持续够了指定时间，则 firing，随后会以一定频率想外部发送告警通知，直到本条告警被解除，然后会发送一条 resolve 状态的通知。

Prometheus 告警通知只支持 webhook，然后发送到 Prometheus 的 alertmanager 上。alertmanager 会把告警聚合，并发送给通知方。一般对 prometheus 二开会保留 alertmanager 核心功能，并开发通知层的能力，例如通知通过邮件，电话等方式发送。

### 4. 使用 Prometheus

使用 Prometheus 指，我们查询 Prometheus 中的数据。存储在 TSDB 中的数据，均为指标，而我们查询数据则是通过 PromQL。虽然实际使用中，我们主要用 Grafana 来查询，但仍然离不开 PromQL，PromQL 就是 Prometheus 的 SQL。

#### 4.1 数据模型

- 时间序列 time-series<br>
  所有数据存储在时间序列数据库里，而每一条时间序列由指标名称(Metrics Name)以及一组标签(键值对)唯一标识。

  ```
  ^
  |   · · · · · · · · ·   · · · · · · node_cpu{cpu="cpu0",mode="idle"}
  |   · · · · · · · · · · · · ·   · · node_cpu{cpu="cpu0",mode="system"}
  |   · · · · · · ·     · · · ·   · · node_load1{}
  |
  <--------------时间-------------->
  ```

- 样本 sample<br>
  在时间序列中的每一个点称为一个样本（sample），样本由以下三部分组成：<br>

  - 指标（metric）：指标名称和描述当前样本特征的 labelsets；<br>
  - 时间戳（timestamp）：一个精确到毫秒的时间戳；<br>
  - 样本值（value）： 一个 folat64 的浮点型数据表示当前样本的值。

  ```
  <--------------- metric ---------------------><---timestamp--><-value->
  http_request_total{status="200", method="GET"}@1434417560938 => 94355
  ```

  实际的样本数据：
  `http_request_total{status="200", method="GET"}@1434417560938 94355`

- 指标(metric)<br>
  其中指标名称(Metrics Name)可以反映被监控样本的含义。例如，http_requests_total — 表示当前系统接收到的 HTTP 请求总量。相同的指标名称配合上不同的标签，会形成不同的时间序列。如，http_requests_total{method="POST"}和 http_requests_total{method="GET"}表示两个不同的时间序列。
  如下为一个指标：

  ```
  <--metric name -->{<label name>=<label value>, ...}
  prometheus_http_requests_total{status="200", method="GET"}
  ```

  指标分为四种类型：

  - counter(计数器)：只增不减<br>
    如：请求次数，任务完成数
  - Gauge（仪表盘）：可增可减
    如：CPU 使用率
  - Histogram（直方图）：查看数据分布<br>
    例如，io_namespace_http_requests_latency_seconds_histogram 为一个 http 请求的耗时

    ```
    // 1. 样本的值分布在 bucket 中的数量，命名为 <basename>_bucket{le="<上边界>"}。解释的更通俗易懂一点，这个值表示指标值小于等于上边界的所有样本数量。
    // 在总共2次请求当中。http 请求响应时间 <=0.025 秒 的请求次数为0
    io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="7.5",} 2.0
    // 在总共2次请求当中。http 请求响应时间 <=10 秒 的请求次数为 2
    io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="10.0",} 2.0
    io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="+Inf",} 2.0
    // 2. 所有样本值的大小总和，命名为 <basename>_sum
    // 实际含义： 发生的2次 http 请求总的响应时间为 13.107670803000001 秒
    io_namespace_http_requests_latency_seconds_histogram_sum{path="/",method="GET",code="200",} 13.107670803000001
    // 3. 样本总数，命名为 <basename>_count
    // 实际含义： 当前一共发生了 2 次 http 请求
    io_namespace_http_requests_latency_seconds_histogram_count{path="/",method="GET",code="200",} 2.0
    ```

    通过 le 标签来反应不同区间内样本的个数。
    Histogram(直方图)返回的是每组的数据

  - Summary（摘要）：记录总体大小<br>
    如：指标 get_info_duration_seconds 表示 get_info 接口的响应时间
    ```
        // 样本分布情况
        get_info_duration_seconds{quantile="0.5"} 0.012352463
        get_info_duration_seconds{quantile="0.9"} 0.014458005
        get_info_duration_seconds{quantile="0.99"} 0.017316173
        // 样本值的大小总和
        get_info_duration_seconds_sum 2.888716127000002
        // 样本总数
        get_info_duration_seconds_seconds_count 216
    ```
    从上面的样本中可以得知当前 get_info 操作的总次数为 216 次，耗时 2.888716127000002s。其中中位数（quantile=0.5）的耗时为 0.012352463，9 分位数（quantile=0.9）的耗时为 0.014458005s。这就是我们经常用于衡量服务响应延迟的 pct50 90 99。
    但是 Summary（摘要）是不能看到极端的情况，例如 100 个请求里有 1 个耗时是 1s,则在 pct99 中无法体现(则用 Histogram 直方图)。
    Summary(摘要)返回的是计算后的值。

> PS:<br>
> up 指标及两个特殊的 label: job, instance
> up 是 Prometheus 内部一个 metric, 它是个 Gauge 类型的 metric， 用于记录 prometheus 采集任务的状态。
> up{instance="localhost:9090", job="prometheus"} up 值 =1，表示采样点所在服务健康；否则，网络不通, 或者服务挂掉了

#### 4.2 PromQL

### 引用

> https://github.com/lichuan0620/k8s-sre-learning-notes/blob/master/prometheus/PROM-101.md > https://www.volcengine.com/docs/6731/177124 > [Prometheus: Up & Running](https://www.oreilly.com/library/view/prometheus-up/9781492034131/) > [Gorilla 论文](http://www.vldb.org/pvldb/vol8/p1816-teller.pdf)
