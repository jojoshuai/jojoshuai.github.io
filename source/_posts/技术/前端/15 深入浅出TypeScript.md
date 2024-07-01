---
title: 15 深入浅出TypeScript
date: 2024-08-01 16:40:41
categories:
  - 前端
tags:
  - 前端
  - typescript
---

# 深入浅出TypeScript
> from https://bytetech.info/videos/set/7193722320643424311/7199477855976161336

## 1 why learn

## 2 TS基础
### 2.1 基础类型
1. boolean、number、string
2. enum
3. any、unknown、void
4. never
5. 数组类型 []
6. 元组类型 tuple
### 2.2 函数类型
定义：TS定义函数类型时要定义输入参数类型和输出类型
输入参数：参数支持可选参数和默认参数
输出参数：输出可以自动推断，没有返回值时，默认为void类型
函数重载：名称相同但参数不同，可以通过重载支持多种类型
### 2.3 接口
定义：接口是为了定义对象类型
特点：
    - 可选属性？
    - 只读属性：readonly
    - 可以描述函数类型
    - 可以描述自定义属性
### 2.4 类
定义：基本同js
特点：
    - 增加了public,private,protected修饰符
    - 抽象类：
        - 只能被继承，不能被实例化
        - 作为基类，抽象方法必须被子类实现
    - interface约束类，使用implements关键字

## 3 TS进阶
### 3.1 高级类型
1. 联合类型 |
2. 交叉类型 &
3. 类型断言
4. 类型别名 type vs interface
### 3.2 泛型
```ts
funcion print<T>(arg:T):T {
    console.log(arg)
    return arg
}

print<string>('hello') // 定义T为strign

print('hello') // TypeScript类型推导，自动推导类型为string

```


## 4 实战和工程向

### 4.1 声明文件
- declare  第三方库需要类型声明文件
- .d.ts  声明文件定义
- @types  三方库TS类型包
- tsconfig.json  定义TS的配置