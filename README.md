# 知识体系

### JavaScript

- [数据类型](#数据类型)
  - [有哪些基本数据类型](#有哪些基本数据类型)
    - [Symbol](#symbol)
    - [数据存储机制](#数据存储机制)
    - [object联想到创建对象的步骤](#object联想到创建对象的步骤)
  - [判断数据类型](#判断数据类型)
    - [各种数据的判断](#各种数据的判断)
      - [判断引用数据类型](#判断引用数据类型)
      - [判断基本数据类型](#判断基本数据类型)
    - [==和===的区别](#和的区别)
      - [===](#)
      - [==](#-1)
    - [拷贝](#拷贝)
      - [赋值](#赋值)
      - [浅拷贝](#浅拷贝)
      - [深拷贝](#深拷贝)
      - [浅拷贝的实现方式：](#浅拷贝的实现方式)
      - [深拷贝的实现方式：](#深拷贝的实现方式)
    - [面试题](#面试题)
      - [null和undefined的区别](#null和undefined的区别)
      - [null是对象吗](#null是对象吗)
      - [基本数据类型和复杂数据类型的区别](#基本数据类型和复杂数据类型的区别)







2. 

2. 对象继承

  - 原型和原型链

  - 函数执行

    - 执行上下文
    - 执行上下文栈、变量对象、作用域链、this、执行上下文栈、变量对象、闭包
    - 创建对象和继承
    - 变量提升
    - v8垃圾回收
    - this
      - call、apply、bind原理及实现

  - oop编程思想

3. ES6
  - let、const
  - 箭头函数
  - Set、Map、WeakSet和WeakMap

4. 异步
  - Promise原理及实现
  - Generator、async和await
  - event loop、宏任务与微任务

5. 模块化
  - CommonJS和ES6 module
  - AMD和CMD区别（比较旧的可以忽略）

### CSS

- 选择器及优先度
- rem、em、vh、vw和px的关系，移动端适配
- 浮动和清除浮动
- 盒子模型
- flex
- 各种布局
- 层级上下文

### 浏览器

- 缓存策略：协商缓存和强缓存
- 页面的渲染过程
- 页面性能优化（展开就不少了，首屏加载，节流防抖）

### 计算机网络

- http
- http2
- https
- 握手
- GET、POST
- TCP/IP

### React

- react执行过程
- react setState执行机制
- react事件机制
- Class的生命周期（16.3和16.4前后差别）
- 高阶组件
- Hook是什么、为什么及怎么样api
- react-router
- Context
- redux、mobx和saga
- react fiber为解决什么问题以及相关概念

### 数据结构与算法

- 基本数据结构
  - 数组
  - 队列和栈
  - 链表
  - 二叉树
  - hash
  - 堆
- 常见算法
  - 深度和广度遍历
  - 各种排序（冒泡、选择、插入、归并、快排等）
  - 二分查找
  - 双指针滑动窗口
  - 回溯
  - 动态规划
  - 贪心

### Webpack

- 使用和基本概念
  - 了解loader、plugin，并掌握一些基本常用内容
  - 了解babel
- 优化
  - 体积优化：tree shaking、按需引入、代码切割
  - 打包速度优化：缓存、多线程打包、优化打包路径
- 原理
  - webpack构建步骤
  - webpack HMR原理
  - 从零实现webpack热更新HMR
  - 写一个插件
  - 写一个loader
- 项目参考
  - le-cli
  - fe-workflow，涉及了初始化项目、打包、测试、联调、质量把控、上线、回滚、线上监控（性能监控、异常监控）等等