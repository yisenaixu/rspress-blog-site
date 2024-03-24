### 创建Store

```js
const store = createStore(rootReducer)
```

### 组合Reducers

```js
const rootReducer = combineReducers({
  // 定义一个名为`todos`的顶级状态字段，由`todosReducer`处理
  todos: todosReducer,
  filters: filtersReducer
})

export default rootReducer
```

### 中间件

> **Middleware 围绕 store 的 `dispatch` 方法形成管线**

#### 用例

- 将某些内容记录到控制台
- 设置定时
- 进行异步 API 调用
- 修改 action
- 暂停 action，甚至完全停止

```js
const middlewareEnhancer = applyMiddleware(print1, print2, print3)

// 将 enhancer 为第二参数，因为没有 preloadedState
const store = createStore(rootReducer, middlewareEnhancer)
```

### 异步Action实现   中间件React-thunk

```js
// thunk函数
async function fetchTodos(dispatch, getState) {
  const res = await api
  dispatch(action)
  // getState()获取更新后的state
  const stateAfter = getState()
}
```

