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
2. 两个不同类型的元素产生不同的树
3. 开发者可以通过key暗示哪些子元素可以在不同的渲染下保持稳定

> 根据第一条规则，只对同级元素进行diff，当newChild类型为object，number，string时，表示更新后同级只有一个元素，为单节点Diff，如果类型为Array，iterator，则表示同级有多个元素，为多节点diff

## 单节点Diff

### 流程

![diff](https://s2.loli.net/2024/03/19/YWcAgoal5C9bKGH.png)

##### 判断节点是否可以复用

1. 判断key是否相同
2. 判断type是否相同

## 多节点Diff

### 元素可能发生的情况

#### 更新

#### 插入 删除

#### 移位

### 两轮遍历

情况有优先级，更新发生的更多，

第一轮 通过key，type确认是否可复用，处理更新，不可复用会跳出循环

第二轮 根据遍历情况 

- newchildren 和 oldfiber 都遍历完，说明只需要处理更新
- newchildren遍历完，oldfiber没有，说明有删除，对oldfiber元素进行删除处理
- oldfiber遍历完，newchildren没有，说明有插入，对newchildren元素标记为插入
- 都没遍历完，说明有移位，此处有特殊算法，记录最后可复用节点在oldfiber中的索引，查询newchildren元素是否在oldfiber中，是则得到索引，比较索引值与最后可复用节点索引值，小于则移位，大于则不需要以为，更新最后可复用节点索引值。





