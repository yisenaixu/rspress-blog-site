## useSelector

从store中读取state

```js
const todo = useSelector(state => state.todo)
```

## UseDispatch

返回store的dispatch方法

```js
const dispatch = useDispatch()
```

## Provider

透传store，让上面的hooks可以读取到store