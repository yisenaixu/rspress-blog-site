# 类组件生命周期（v17）

![image.png](https://s2.loli.net/2024/04/05/Qdr2DLH8JpZjuVM.webp)

## 挂载阶段

### constructor 

初始化state，props，绑定事件处理函数

### componentWillMount（v17后不推荐，被弃用）

挂载前调用

### getDerivedStateFromProps（v17新增） 

拿到新的props和旧的state，返回计算后的state或者null，也就是在收到新的props时触发（）

> 感觉与useEffect添加props依赖项调用updateState函数类似，某个state依赖于props变化而变化的使用？

**render阶段之前**

### componentDidMount （commit阶段后）

挂载后调用

> useEffect，依赖为空数组，实现类似效果，只在挂载时触发一次

## 更新阶段

### componentWillReceiveProps(nextProps)（v17后弃用）

props变化后调用

### getDerivedStateFromProps（v17新增）

### shouldComponentUpdate

判断是否更新，性能优化相关，useMemo

### componentWillUpdate(v17弃用)

更新前调用

### getSnapshotBeforeUpdate（v17新增）

进入commit阶段前调用，此时真实dom元素还没有更新

**commit阶段后**

### componentDidUpdate

更新后调用

> useEffect无依赖每次render后都会触发，把挂载后通过useRef取消第一次调用实现类似效果

## 卸载阶段

### componentWillUnmount

卸载前调用

> useEffect第一个函数参数的返回值实现类似效果

# 函数式组件类生命周期

函数式组件不存在生命周期，也没有状态

但通过Hooks可以实现类组件的状态以及类似生命周期钩子，基本可以覆盖类组件的生命周期钩子



![img](https://s2.loli.net/2024/04/05/V3dNJiZhMPTIofF.png)

 

```js
function ScrollView({row}) {
  let [isScrollingDown, setIsScrollingDown] = useState(false);
  let [prevRow, setPrevRow] = useState(null);
  
  // 类getDerivedStateFromProps效果  
  if (row !== prevRow) {
    // Row 自上次渲染以来发生过改变。更新 isScrollingDown。
    setIsScrollingDown(prevRow !== null && row > prevRow);
    setPrevRow(row);
  }

  return `Scrolling down: ${isScrollingDown}`;
}
```

# 类组件与函数式组件的区别

- 相同功能类组件代码量多余函数组件代码
- 函数式组件没有生命周期和状态，通过引入Hooks实现了类似功能
- 类组件需要在生命周期函数中添加逻辑，一个组件多个业务功能在同一个生命周期函数中，臃肿且难以复用
- 类组件使用this，有this指向的问题