---
title: python基础：类
date: 2024-06
categories:
  - python
---

- 类中的\_\_xx 不能直接访问的原因：
  - 不能直接访问**name 是因为 Python 解释器对外把**name 变量改成了\_Student**name，所以，仍然可以通过\_Student**name 来访问\_\_name 变量
- 类中的\_xx 只是不推荐访问，但是其实可以直接访问
