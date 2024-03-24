# Diff算法

> 为了防止概念混淆，这里再强调下
>
> 一个`DOM节点`在某一时刻最多会有4个节点和他相关。
>
> 1. `current Fiber`。如果该`DOM节点`已在页面中，`current Fiber`代表该`DOM节点`对应的`Fiber节点`。
> 2. `workInProgress Fiber`。如果该`DOM节点`将在本次更新中渲染到页面中，`workInProgress Fiber`代表该`DOM节点`对应的`Fiber节点`。
> 3. `DOM节点`本身。
> 4. `JSX对象`。即`ClassComponent`的`render`方法的返回结果，或`FunctionComponent`的调用结果。`JSX对象`中包含描述`DOM节点`的信息。
>
> `Diff算法`的本质是对比1和4，生成2。

## React算法限制

diff操作本身有性能损耗，算法复杂度O(n3),为了降低算法复杂度，做出以下限制

1. 只对同级元素进行diff
2. 两个不同类型的元素产生不同的数
3. 开发者可以通过key暗示哪些子元素可以在不同的渲染下保持稳定

> 根据第一条规则，只对同级元素进行diff，当newChild类型为object，number，string时，表示更新后同级只有一个元素，为单节点Diff，如果类型为Array，iterator，则表示同级有多个元素，为多节点diff

## 单节点Diff

### 流程

![diff](https://s2.loli.net/2024/03/19/YWcAgoal5C9bKGH.png)

##### 判断节点是否可以复用

1. 判断key是否相同
2. 判断type是否像提供

## 多节点Diff

### 流程



