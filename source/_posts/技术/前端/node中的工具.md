---
title: node中的工具
date: 2024-08-15 20:06:17
categories:
  - 前端
tags:
  - node
  - nvm
  - nrm
  - npm
  - pnpm
  - yarn
---

## node

### nvm node 的版本管理工具

> nvm（node.js version management），是一个 nodejs 的版本管理工具
>
> 1. 为了解决 node.js 各种版本存在不兼容现象 可以通过它可以安装和切换不同版本的 node.js。【可同时在一个环境中安装多个 node.js 版本（和配套的 npm）
> 2. 官方也推荐使用 nvm 直接安装和管理 node，见 https://nodejs.org/zh-cn/download/package-manager

#### 使用

```
// 因为墙的问题，可以选择使用下面的命令处理网络问题
npm get registry  //查看当前的镜像源
npm config set registry https://registry.npm.taobao.org/  //设置淘宝镜像源
npm config set registry http://www.npmjs.org/ //切换回官方镜像源

// # installs nvm (Node Version Manager) --> 详细见github仓库
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

// 设置阿里镜像源
nvm npm_mirror https://npmmirror.com/mirrors/npm/
nvm node_mirror https://npmmirror.com/mirrors/node/
// 设置腾讯云镜像源
nvm npm_mirror http://mirrors.cloud.tencent.com/npm/
nvm node_mirror http://mirrors.cloud.tencent.com/nodejs-release/
```

### nrm 镜像源管理工具

    nrm(npm registry manager )是npm的镜像源管理工具，有时候国外资源太慢，那么我们可以用这个来切换镜像源。

#### 1. 全局安装：

`npm install -g nrm`

#### 2. 使用

```
nrm ls // 列出可用源
nrm ls      //查看当前可用镜像源
nrm use cnpm  //切换到指定镜像源
```

## npm node 的依赖包管理器

- npm

  node 官方的依赖管理器

- yarn

  另一套更流行的包管理器，增加了包管理能力
  npm 安装是非确定性的，程序包没有签名，并且 npm 除了做了基本的 SHA1 哈希之外不执行任何完整性检查，这给安装系统程序带来了安全风险。

- pnpm

  继承了 npm 和 yarn 的优点的包管理器，同时更快(硬链接和符号链接来避免复制所有本地缓存源文件)

> 关于三者的区别见：https://pnpm.io/zh/feature-comparison

### pnmp

1. 安装

2. 配置
   设置存储地址： `pnpm config set store-dir /path/to/.pnpm-store`，如果没有配置 store ，那么 pnpm 将自动在同一磁盘上创建 store
