## Redix Toolkit

> **Redux Toolkit 是编写 Redux 应用程序逻辑的标准方式**Redux Toolkit 包含了对于构建 Redux 应用程序至关重要的包和函数。它构建在我们建议的**最佳实践**中，**简化**了大多数 Redux 任务，避免了常见错误，使得编写 Redux 应用程序更容易了。

### configureStore

```js
const store = configureStore({
  reducer: {
    // 定义一个名为 `todos` 的顶级 state 字段，值为`todosReducer`
    todos: todosReducer,
    filters: filtersReducer
  }
})
```

> - 将 `todosReducer` 和 `filtersReducer` 组合到根 reducer 函数中，它将处理看起来像 `{todos, filters}` 的根 state
> - 使用根 reducer 创建了 Redux store
> - 自动添加了 “thunk” middleware
> - 自动添加更多 middleware 来检查常见错误，例如意外改变（mutate）state
> - 自动设置 Redux DevTools 扩展连接