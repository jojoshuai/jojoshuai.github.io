---
title: iterm2自定义
date: 2024-05-14 14:53:33
categories:
  - 开发工具
tags:
  - iterm
---

# iterm2 自定义

### roboto mono 字体

下载地址
https://github.com/googlefonts/RobotoMono/tree/main/fonts/variable

### Spaceship Prompt 主题

安装了 zsh，并安装了 oh-my-zsh 后，可以使用本主题，本主题用于现实当前 pwd 的位置，包括：用户名、主机名、目录、git 分支

主题项目地址：https://github.com/spaceship-prompt/spaceship-prompt
前提：

1. echo $ZSH_VERSION #> 5.8.1
2. [Powerline Font](https://github.com/powerline/fonts) or [Nerd Font](https://www.nerdfonts.com/) (even better)必须被安装在终端里

安装：

1. 下载源码：git clone --depth=1 https://github.com/spaceship-prompt/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt"
2. 修改主题：
   1. vim ~/.zshrc
   2. 修改 ZSH_THEME="spaceship-prompt/spaceship"
3. `source ~/.zshrc` 使之生效
4. 处理表情为  的问题: iterm 里 settings->Profiles->Text->字体选择 Roboto Mono for Powerline。PS：任意的 For Powerline 的字体都能使特殊字符显示
