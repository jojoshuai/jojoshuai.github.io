---
title: python虚拟环境管理 - virtualenv(Not Recommand)
date: 2024-08-14 23:23:05
categories:
  - python
tags:
  - python
  - 虚拟环境管理工具
  - virtualenv
---

# python 虚拟环境管理工具 virtualenv 工具

> ❤️ Python 包隔离工具，主要来隔离不同 Python 项目的依赖 <br>
> virtualenv 支持全部 python 版本

## virtualenv 的用途

在指定目录下安装虚拟的 python 环境，指定 python 的版本<br>
用于解决不同项目依赖的 python 版本不同的问题

## 安装

    ```bash
    pip3 install virtualenv
    ```

## 使用方法

> 基本同 venv
> (venv 是参考 virtualenv 实现的，我们可以简单的理解为 virtualenv 是 venv 的超集)

- 创建虚拟环境

  ```bash
  #  ⚠️ path-to-your-env-dir是一个python环境的地址，并不是你的项目地址
  #     所以项目代码跟虚拟环境目录可以是无关联的，使用时只需要激活对应虚拟环境即可
  # ⚠️ 不指定python版本的情况下自动创建virtualenv所对应的版本
  #     使用-p指定版本 virtualenv env3.9 -p python3.9
  virtualenv path-to-your-env-dir
  # bin下存放python和pip命令的目录
  # lib/python3.12/site-package下存放virtualenv的软件包管理目录
  ls -al
  ```

- 启用虚拟环境
  ```bash
  source path-to-your-env-dir/bin/activate
  ```
  source 后，在控制台的命令行提示符前面会出现虚拟环境名称
  ```
  ➜  ~ source python3.12/bin/activate
  (python3.12) ➜ ~
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
- 继承系统中已安装的包 --system-site-packages
  ```
  virtualenv --system-site-packages env3.12
  ```

## 总结

- 功能基本跟 venv 一致；相比 venv 多了指定版本的功能
- 支持 python2
- 可以用 virtualenvwrapper 来将虚拟环境集中管理
