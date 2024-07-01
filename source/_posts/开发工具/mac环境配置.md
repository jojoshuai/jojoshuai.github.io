---
title: mac环境配置
date: 2024-08-16 15:02:33
categories:
  - mac
tags:
  - mac
---

# Mac 变生产力

## 必备

- brew
- docker
  - 不是开发也可以用 docker 来启动一些有用的应用或项目

## develop

- git
- vscode
- zsh
  - autojump
  -

## python

1. pyenv

2.

# 删除特定文件

```shell
  # 删除用户为 502 的所有文件
  find . -user 502 -type l -delete # 软链接
  find . -user 502 -type d -delete # 目录
  find . -user 502 -type f -delete # 文件
```
