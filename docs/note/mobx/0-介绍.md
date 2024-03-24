# 介绍

mobx是react实现状态管理的一种方案，与redux不同。mobx是一个响应式编程库，看文档觉得和vue有点类似，原理也基本一致，`Object.defineProperyty`  或 `Proxy`

## 执行流程

<img src="https://s2.loli.net/2024/03/11/yaIL6j3DcePAVsq.png" alt="Action, State, View" style="zoom:50%;" />

直接借用官网上的图，可以简单概括为

1. 触发event事件执行Actions
2. Actions改变observableState
3. state改变引起Derivations(派生)变化，也就是computed属性，根据computed属性依赖的state变化后的值重新计算computed属性的值。
4. state更新后触发Reactions，响应state变化做出附加操作

## 核心概念

综合体验比redux要简单(学习上手难度)，没有rudux较为复杂且多的概念

### observable

使用observable将接收到的值包装为**可观察对象**

类似于vue中的data

### computed

基于现有状态(**可观察对象**)衍生出的值

类似于vue中的computed

### reaction(autorun)

监听现有状态(**可观察对象**)，发生变化则执行对应操作(缓存数据，打印日志)

类似于vue中的watch





