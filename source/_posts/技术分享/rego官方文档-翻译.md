---
title: rego官方文档-翻译
date: 2024-09-18 10:42:17
categories:
  - 技术分享
tags:
  - rego
---

# 1. 概览

# 2. 例子

# 3. Rego

## 3.1

## 3.2

## 3.3

## 3.4 迭代 Iteration

## 3.5 规则 Rules

Rego 支持你使用规则(Rules)来封装并重用逻辑。规则(Rules)只是`if-then`逻辑语句。规则(Rules)可以是 complete rule 或 partial rule

### 3.5.1 完整规则 Complete Rules

完整规则 是 赋单个值给变量的 `if-then`逻辑语句。例如:

```rego
any_public_networks := true if {
    some net in input.networks # some network exists and..
    net.public                 # it is public.
}
```

每个规则由 Head 和 body 组成。在 Rego 中，如果 Rule Body 对于某些变量赋值集为 True，则我们说规则头为 True。在上面的例子中，`any_public_networks := true` 是 head，`some net in input.networks; net.public`是 body

你可以像其他值一样的查询方式，来查询由规则生成的变量。

```
any_public_networks
# 输出 true
```

所有的 Rule 生成的值，都可以通过全局变量`data`来查询：

```
data.example.rules.any_public_networks
##
## 您可以通过使用绝对路径引用加载到 OPA 中的任何Rule来查询其value。
## Rule的路径总会是：data.<package-path>.<rule-name>
```

你可以省略 Rule Head 中的`= <value>`，这个值默认是`true`。所以你可以这样重写上面的规则：

```
any_public_networks if {
    some net in input.networks
    net.public
}
```

在定义常量是，省略规则体(Rule Body)。当你省略了 Rule Body 时，它默认为`true`，因为 rule body 是 true，所以 rule head 也为 true/defined.

```
package example.constants

pi := 3.14
```

这样定义常量，可以像其他值一样被查询。

```
pi > 3
## true
```

如果 opa 没有找到满足 规则体 的变量指定，我们说这个规则是`undefined`。例如，如果 input 提供给 opa 的值没有包括 public network，那么`any_public_networks`会是`undefined`(这和`false`不一样)。下面给 opa 的输入中：没有 network 是 public 的，故最后值会是`undifined`

```
### 规则
any_public_networks if {
    some net in input.networks
    net.public
}

### 输入
{
    "networks": [
        {"id": "n1", "public": false},
        {"id": "n2", "public": false}
    ]
}

### 查询
any_public_networks
### 输出
undefined decision
```

### 3.5.1 部分规则 Partial Rules

部分规则是指：生成了一组值(a set of values)并将该 set 给一个变量的`if-then`语句。例如：

```
public_network contains net.id if {
    some net in input.networks # some network exists and..
    net.public                 # it is public.
}

```

`public_network contains net.id if`是规则头，`some net in input.networks; net.public`是规则体。你可以使用查询其他变量的方式来查询整组值：

```
### 查询
public_network
### 输出
[
  "net3",
  "net4"
]

```

使用`some ... in ...`表达式可以变量整组值

```
some net in public_network # 遍历
### 输出
+--------+
|  net   |
+--------+
| "net3" |
| "net4" |
+--------+

```

使用文字或绑定变量，您可以通过 `... in ...` 检查该值是否存在于集合中：

```
"net3" in public_network
### 输出
true
```

你也可以遍历这组值，通过使用一个变量来引用 set 中的 element 的方式

```
some n; public_network[n]
### 输出
+--------+-------------------+
|   n    | public_network[n] |
+--------+-------------------+
| "net3" | "net3"            |
| "net4" | "net4"            |
+--------+-------------------+
```

你可以使用和如下相同的语法来检查一个值是否存在这个 set 中：

```
public_network["net3"]
### 输出
"net3"
```

## 3.6 合在一起 Putting It Together

# 4. 运行 OPA

# 5. 下一步
