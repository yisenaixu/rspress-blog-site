## grid 栅格布局

flex布局以轴布局，指定项目相对于轴线的位置

grid布局划分行列，划分一个个单元格，指定项目对应的单元格，以及项目在单元格内的对其方式，以及所有单元格相对于容器的布局方式

### 概念

#### 容器 

赋予display：grid的盒子

#### 项目

grid盒子下的子盒子，高宽有父盒子进行划分，也可以自己指定（但会影响同行或列的宽高，一般没必要）

### 行列划分

##### grid-template-[columns\rows]  值可以是具体的数值，也可以是fr，或者minmax()自适应

特别的，可以指定项目的行列属性，让项目在占据容器任意的区域

##### grid-row-start grid-row-end grid-column-start grid-column-end

以上属性赋予项目可以指定项目的行列开始与结尾，可以让项目占据任意位置及数量的容器内区域，其他元素则会自动按照顺序补充剩下的区域

```css
.item-c {
  grid-column: 3 / span 2;
  grid-row: third-line / 4;
}
 
```

![Example of grid-column/grid-row](https://css-tricks.com/wp-content/uploads/2018/11/grid-column-row.svg)

你有两种写法对于这种属性

- 输入开始行/列和结束行/列（数字英文都可以）
- 输入开始行/列，和占据几行

##### 此时需要考虑，如果你项目数量不够，却排在了后排/列，中间没有项目自动补充，那么此时中间项目的宽高是怎样的，如果他在划分的行列里，显示宽高是行列高度，如果超出呢，那么应该是0，可以通过grid-auto-rows/columns去规定这些项目的宽高

```css
.container {
  grid-template-columns: 60px 60px;
  grid-template-rows: 90px 90px;
}
.item-a {
  grid-column: 1 / 2;
  grid-row: 2 / 3;
}
.item-b {
  grid-column: 5 / 6;
  grid-row: 2 / 3;
}
```

![Example of implicit tracks](https://css-tricks.com/wp-content/uploads/2018/11/grid-auto-columns-rows-02.svg)

#### 对齐方式

##### justify/align/place-content/items/self

##### justify 横向 align纵向 place横纵

##### content

容器操纵所有项目的对齐方式

容器划分行列后，项目的总大小仍没有占满容器，此时容器设置content让所有项目在容器水平垂直居中

```css
.container {
  align-content: center;    
}
```

![Example of align-content set to center](https://css-tricks.com/wp-content/uploads/2018/11/align-content-center.svg)

##### items/self

分别由容器和项目使用，items可以看作是复数的self操作，操纵的是项目与被容器划分的区域的对其关系，将容器划分的行列单元格默认为虚拟的，项目是真实对应单元格的内容

默认值：stretch ，表现为项目占满划分单元格的宽高

如果划分单元格是200px宽度,我给项目500px宽度，让他水平居中，会是这样，所以说容器的划分并不会实际控制项目的宽高，项目只是关联于容器划分的单元格

![image-20240407211735532](https://s2.loli.net/2024/04/07/63cNSI2Km5qlHAp.png)





```js
.parent {
            width: 600px;
            height: 600px;
            display: grid;
            grid-template-columns: repeat(3,1fr);
            justify-items: center;
            align-items: center;
            gap: 10px;
}
.left {
          width: 100%;
          height: 100%;
          background-color: aqua;
}
.right {
          background-color: aqua;
}
```

 <img src="https://s2.loli.net/2024/04/07/CMdV74kIjOpY28i.png" alt="image-20240407212119218" style="zoom:33%;" /><img src="https://s2.loli.net/2024/04/07/jODhspZai8dQblI.png" alt="image-20240407212127906" style="zoom:33%;" />         

可以看到右边是水平垂直居中的但高度没有占满，左边宽高是占满的但是内部数字没有水平垂直居中。

#### 间距 gap

每个元素间的间距，会减少单元格的宽高

#### 区域 area

没实际应用过，后续有使用再补充