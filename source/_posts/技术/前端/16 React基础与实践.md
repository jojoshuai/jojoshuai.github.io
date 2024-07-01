---
title: 16 React基础与实践
date: 2024-08-01 16:40:41
categories:
  - 前端
tags:
  - 前端
  - typescript
---

# React 基础与实践

> https://bytetech.info/videos/set/7193722320643424311/7199833071615475767

## 1. React 简介和特性

### 1.1 React 特点

- 声明式
- 组件化
- 跨平台编写

### 1.2 React 哲学

**等待资源加载时间**和**大部分情况下浏览器单线程执行**是影响 web 性能的量大主要原因

- 等待资源加载时间: React.Lazy React.Suspense ErrorBoundary
- 浏览器单线程执行: 异步更新 时间切片 ReactFiber

### 更新流程

Scheduler 调度器
