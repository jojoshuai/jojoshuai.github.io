---
title: python虚拟环境管理 - venv
date: 2024-08-14 16:40:41
categories:
  - python
tags:
  - python
  - 虚拟环境管理工具
  - venv
---

# python 虚拟环境管理工具 venv 工具

> ❤️ Python 包隔离工具，主要来隔离不同 Python 项目的依赖 <br>
> venv 只能在**3.3**以上版本推荐使用 <br>
> 3.3 以前使用 virtualenv

## venv 的用途

在指定目录下安装虚拟的 python 环境，指定 python 的版本<br>
用于解决不同项目依赖的 python 版本不同的问题

## 安装

略
python3.3 之后自带

## 使用方法

- 创建虚拟环境

  ```bash
  # ⚠️ path-to-your-venv是一个python环境的地址，并不是你的项目地址
  #     所以项目代码跟虚拟环境目录可以是无关联的，使用时只需要激活对应虚拟环境即可
  # ⚠️ 如果此处python3的具体python版本是python3.12，那么创建的虚拟环境也是python3.12的环境
  #     所以我们需要先安装3.3及以上版本的python，才能用venv创建和使用此版本的虚拟环境
  python3 -m venv path-to-your-venv
  ```

- 启用虚拟环境
  ```bash
  source path-to-project/bin/activate
  ```
  source 后，在控制台的命令行提示符前面会出现虚拟环境名称
  ```
  ➜  ~ source venv-python12/bin/activate
  (venv-python12) ➜ ~
  ```
- 在虚拟环境激活时 使用 python
  ```
  python --version  # 可以直接用python命令，而不是python3
  # 输出
  # Python 3.12.4
  ```
- 退出当前虚拟环境
  ```
  deactivate
  ```

## 优劣

- 优势
  - python 自带的，无须安装
  - 满足基本功能
