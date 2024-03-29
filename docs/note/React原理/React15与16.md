> `React`从 v15 升级到 v16 后重构了整个架构。本节我们聊聊 v15，看看他为什么不能满足**快速响应**的理念，以至于被重构。

## React15架构

### 协调器Reconciler 

找出变化组件

- 调用函数组件、或 class 组件的`render`方法，将返回的 JSX 转化为虚拟 DOM
- 将虚拟 DOM 和上次更新时的虚拟 DOM 对比
- 通过对比找出本次更新中变化的虚拟 DOM
- 通知**Renderer**将变化的虚拟 DOM 渲染到页面上

### 渲染器Renderer

在每次更新发生时，**Renderer**接到**Reconciler**通知，将变化的组件渲染在当前宿主环境。

### 缺点

更新为**递归**执行，无法中断，如果让他中断，则后续步骤无法执行，用户看到更新不完全的dom，因此重构

协调器和渲染器交替工作导致如果中断更新不完全

## React16架构

新增调度器(Scheduler)

### 调度器Scheduler

负责调度任务，管理中断，以及选择更需要被高优先级更新的任务进行快速响应。

### 协调器Reconciler 

处理虚拟dom更改为可中断的（实现fiber Reconciler）

不与渲染器交替工作，改为打标记（flag），标记采用二进制，有特别算法

虚拟dom实现，对虚拟dom的遍历对比(fiber树，diff算法)，打标记(flag)

