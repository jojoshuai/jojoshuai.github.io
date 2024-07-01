---
title: python 版本管理工具 - pyenv
date: 2024-08-15 20:06:17
categories:
  - python
tags:
  - pyenv
---

# python 版本管理工具 - pyenv

> https://github.com/pyenv/pyenv
> 需要区别于 venv <br>
> pyenv 使用的场景是： <br>
>
> > 我们有多个项目，每个项目所使用的 python 版本不同，如 A 使用 python2，B 使用 python3.1，C 使用 python3.12 <br>
> > 这时我们需要在系统中安装多个版本的 python <br>

1. pyenv 会从源码安装 python，其每个安装的版本都会在～/.pyenv/version 内 (~/.pyenv 是默认安装路径，可修改)
2. 安装： pyenv install 3.12.1
3. 查看已安装的列表：pyenv versions
4. 查看当前的版本：pyenv version
5. 查看能安装的版本列表：pyenv install -l
6. 版本切换：
   ```
   # 设置全局的 Python 版本，通过将版本号写入 ~/.pyenv/version 文件的方式。
   pyenv global 2.7.3
   # 设置 Python 本地版本，通过将版本号写入当前目录下的 .python-version 文件的方式。通过这种方式设置的 Python 版本优先级较 global 高。
   pyenv local 2.7.3
   # 设置面向 shell 的 Python 版本，通过设置当前 shell 的 PYENV_VERSION 环境变量的方式。这个版本的优先级比 local 和 global 都要高。
   pyevn shell 2.7.3
   # 取消当前 shell 设定的版本。
   pyevn shell --unset
   # 当增删了python版本或pip之后，都应该执行一次本命令
   pyenv rehash
   ```
   - 优先级：shell > local > global
     - ❤️ 在查找 local 级的 pyenv 配置时，会从当前目录开始向上找`.python-version`直到根目录，找不到，才会用 global
