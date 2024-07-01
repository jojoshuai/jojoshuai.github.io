---
title: React基础-JSX
date: 2024-08-01 16:40:41
categories:
  - 前端
  - react
tags:
  - React
  - JSX
---

1. jsx 是 javascript 和 xml(html)的缩写，表示在**js 代码中编写 html 模版结构**，它是 React 中编写 UI 模版的方式
   - 优点：
     - html 的声明式模版写法
     - js 的可编程能力
2. jsx 是 js 的语法扩展，需要解析后才能在浏览器运行

   - jsx --> babel -> html
   - jsx 中可以通过 大括号语法{} 识别 javascript 中的表达式，比如常见的变量，函数调用，方法调用的呢感动呢个

   ```js
   function App() {
     return (
       <div className="App">
         this App
         {/* 使用引号传递字符串 */}
         {" this is message "}
         {/* 识别js变量 */}
         {count}
         {/* 函数调用 */}
         {getName()}
         {/* 方法调用 */}
         {new Date().toString()}
         {/* 使用js对象*/}
         <div style={{ color: "red" }}> here is a div from react</div>
       </div>
     );
   }
   ```

   - 但 if 语句、switch 语句、变量声明属于 语句，不是表达式，不能出现在{}中

3. jsx 中实现列表渲染
