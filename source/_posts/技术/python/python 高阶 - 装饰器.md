---
title: python 高阶 - 装饰器
date: 2024-08-15 20:06:17
categories:
  - python
tags:
  - 装饰器
---

# 装饰器:

> 用于装饰其他函数，即为其他函数增加特定功能的函数

```python
'''太长不看版本'''
# 对于 无参数、无返回值的函数 的装饰器
def decorate(f):
    def inner():
        # do sth
        f()
    return inner
# 对于 有参数、有返回值的函数 的装饰器
def decorate(f):
    def inner(*args, **kwargs):
        # do sth
        res = f(*args, **kwargs)
        return res
    return inner
# 对于 有参数、有返回值的函数 的需要入参的装饰器
def decorate(decorate_params):
    # do sth about decorate_params
    def outer(f):
        def inner(*args, **kwargs):
            # do sth
            res = f(*args, **kwargs)
            return res
        return inner
    return outer
```

## 原则：

- 装饰器不会影响被装饰函数的源码
- 装饰器不能修改被装饰函数的调用方式

## 基本装饰器

> 前提概念：
>
> - 函数即变量
> - 高阶函数
> - 嵌套函数
>
> 装饰器= 高阶函数+嵌套函数

### 高阶函数：将函数作为变量传入并传出

```python
def timer(f):
    print("man, what can i say!")
    f()

def mamba():
    print("mamba out!")

timer(mamba)
```

使用高阶函数可以初步实现功能，但是会影响原来函数的调用方式

### 嵌套函数：在函数内定义函数

```python
def kobe(f):
    def t():
        print("what can i say!")
        f()
    return t

def mamba():
    print("mamba out!")

mamba = kobe(mamba)
mamba()
```

实现了装饰器的逻辑：符合即不修改调用方式，也不修改源码的原则

### python 的一个语法糖@

> 虽然上面实现了，但是整体实现繁琐，所以 python 给了一个语法糖来使得代码变得优雅

```python
def kobe(f):
    def t():
        print("what can i say!")
        f()
    return t

@kobe  # 此处等于执行了: mamba = timer(mamba)
def mamba():
    print("mamba out!")

# mamba = timer(mamba)
mamba()
```

## 装饰带参数的函数

### 直接的解决方式

```python
def kobe(f):
    def t(t): # 嵌套函数的定义需要跟被装饰函数一致
        print("what can i say!")
        f(t) # 调用嵌套函数时传入参数
    return t

@kobe
def mamba(t):
    print("mamba out!", t)

mamba("man")
```

> 缺点是：嵌套函数的定义需要跟被装饰函数一致

### 通过不定量参数将装饰器和被装饰函数解耦

```python
def kobe(f):
    def t(*args, **kwargs):
        print("what can i say!")
        f(*args, **kwargs) # 调用嵌套函数时传入参数
    return t

@kobe
def mamba(a, b, c):
    print("mamba out!", a, b, c)

mamba("M", "A", "N")
```

## 装饰器本身带参数

> 增加一层嵌套，用于接受传入装饰器的参数

```python
import time

def timer(type):
    print(type, 1)
    def outter(f):
        print(type, 2)
        def inner(*args, **kwargs):
            print(type, 3)
            start = time.time()
            f(*args, **kwargs) # 调用嵌套函数时传入参数
            end = time.time()

            print(f'timer: {end-start}')
        return inner
    return outter

@timer(type="min") # 相当于执行了: work = time(type="min")(work)
def work(duration, sth):
    print(f'cost {duration}s, working for {sth}')
    time.sleep(1)
    print("working finish")

work(1, "farm")
'''
> output:
min 1
min 2
min 3
cost 1s, working for farm
working finish
timer: 1.0051038265228271
'''
```

## 装饰有返回值的装饰器

> 增加一层嵌套，用于接受传入装饰器的参数

```python
import time

def timer(type):
    print(type, 1)
    def outter(f):
        print(type, 2)
        def inner(*args, **kwargs):
            print(type, 3)
            start = time.time()
            res = f(*args, **kwargs) # 增加返回值
            end = time.time()
            print(f'timer: {end-start}')
            return res # 增加返回值
        return inner
    return outter

@timer(type="min") # 相当于执行了: work = time(type="min")(work)
def work(duration, sth):
    print(f'cost {duration}s, working for {sth}')
    time.sleep(1)
    print("working finish")
    return "earn ¥10" # 增加返回值
print(work(1, "farm"))
'''
output:
min 1
min 2
min 3
cost 1s, working for farm
working finish
timer: 1.0025570392608643
earn ¥10
'''
```

> 如果返回多个值仍然不用修改代码，因为 python 自动会把多个参数打包成元组给 res，并将 res 解包成多个参数给真正的 work

```python
import time

def timer(type):
    print(type, 1)
    def outter(f):
        print(type, 2)
        def inner(*args, **kwargs):
            print(type, 3)
            start = time.time()
            res = f(*args, **kwargs) # 增加返回值
            end = time.time()
            print(f'timer: {end-start}')
            return res # 增加返回值
        return inner
    return outter

@timer(type="min") # 相当于执行了: work = time(type="min")(work)
def work(duration, sth):
    print(f'cost {duration}s, working for {sth}')
    time.sleep(1)
    print("working finish")
    return "earn ¥10", "happy" # 增加返回值

earn, emotion = work(1, "farm")
print(earn, emotion)
'''
min 1
min 2
min 3
cost 1s, working for farm
working finish
timer: 1.0020017623901367
earn ¥10 happy
'''
```
