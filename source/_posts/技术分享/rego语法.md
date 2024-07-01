---
title: rego语法
date: 2024-06-23 17:18:17
categories:
  - 技术分享
tags:
  - rego
  - opa
---

## 语法

### 基础支持

### 标量值

标量值可以是：字符串、数字、布尔、null

```rego
foo := "bar"
max_height := 42
pi := 3.14
allowed := true
location := null
```

### 综合值

```rego
cube := {"width":3, "height":4, "depth":5}
```

### 对象

对象是无序键值集合。在 rego 中，任何值类型都可以用作对象键。

```rego
# 定义值
ips_by_port := {
    80: ["1.1.1.1", "1.1.1.2"],
    443: ["2.2.2.1"]
}

# 查看
ips_by_port[80] # 查看key为80的值
ips_by_port[port][_] == "2.2.2.1" # 查询port，条件为value为2.2.2.1
ips_by_port # 查看整个值，以json输出

# 空对象
eo := {}
# 查看空对象
eo # {}

```

### 集合

```rego
# 定义
s := {1,2,3}

# 查看
s # 查看整个值，以json输出
s[_] # 列出集合的所有值

# 比较
s == {3, 1, 2} # true

# 空集合
es = set()

# 查看
es # []

# 查看集合长度
count(es) # 0
coutn(s) # 3
```

### 变量

```
sites := [
    {"name": "prod"},
    {"name": "smoke1"},
    {"name": "dev"}
]

```
