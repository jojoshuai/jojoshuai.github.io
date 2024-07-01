---
title: Elaticsearch教程
date: 2024-11-12
categories:
  - elasticsearch
tags:
  - gc
---

# ELK = Elasticsearch + Logstash + Kibana

- Elasticsearch - 数据库
- Logstash - 将数据 同步/上传 至 Elasticsearch
- Kibana - ES 的可视化支持，同时作为查询后台

# Elasticsearch

## 1. 基本概念

1. index 索引 === mysql 表
   - type 在旧版本中对应 mysql 表，index 对应 mysql 库。7.0 之后废弃了 type，故只需要考虑 index = mysql 表
2. Document 文档 === mysql 表中的一行数据
   - 文档 使用 json 格式来存储
   - 文档的格式可以为任意格式(但通常业务中不会这样做)
3. Field 文档字段 === mysql 表中的字段
   - 数值类型（包括: long、integer、short、byte、double、float）
   - text - 支持全文搜索
   - keyword - 不支持全文搜索，例如：email、电话这些数据，作为一个整体进行匹配就可以，不需要分词处理。
   - date - 日期类型
   - boolean
   - 等
   - 其类型会动态映射
4. mapping 映射 === mysql 表的结构定义
   - 根据 mapping 将 json 的对应字段映射为对应类型
