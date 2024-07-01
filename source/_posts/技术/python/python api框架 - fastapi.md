---
title: python api框架 - fastapi
date: 2024-08-15 20:06:17
categories:
  - python
tags:
  - fastapi
---

## 介绍

fastapi 是基于 asgi 协议开发的 api 框架。而 flask，django 等框架都是基于 wsgi 协议开发的。

- asgi 是 Asynchronous Server Gateway Interface，是异步 web 网关
- wsgi 是 Web Server Gateway Interface，是同步 web 网管
- 因为 fastapi 是基于 wsgi 的，所以需要一个对应的服务器；而 uvicorn 则扮演这个角色，uvicorn+fastapi 可实现异步多进程的服务器。
