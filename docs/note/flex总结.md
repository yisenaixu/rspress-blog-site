## Flex 弹性布局

### 思想

是一个容器（父元素，被赋予display：flex的元素）可以操纵内部项目(子元素)宽高以及顺序，更好的填补空余空间。

### 重要的概念

#### 容器 

主要属性： display:flex | flex-deriction | [] -content | []-items

#### 项目

主要属性： flex | []-self

#### 主轴

项目按照主轴依次布置

#### 交叉轴

垂直于主轴

#### content | items | self

控制容器内对其方式的属性，flex布局只有justify-content ，align-items，align-self，align-content

##### align-content,align-items有什么区别

这两个元素都是在容器上的，都是操纵在交叉轴的对其方式，区别在于对单行和多行的处理

###### align-content 所有项目相对于整个容器的高度计算

###### align-items 所有项目相对于他们这一行的高度计算

​                                                                            align-content

<img src="https://s2.loli.net/2024/04/06/u7IAG9Jbsf4hzxr.png" alt="image-20240406224342567" style="zoom:33%;" />

​                                                                             align-items

<img src="https://s2.loli.net/2024/04/06/LIGnFuZsRaKcr28.png" alt="img" style="zoom: 33%;" />

```css
        .flex {
            display: flex;
            flex-wrap: wrap;
            width: 600px;
            height: 200px;
            background-color: black;

            /* align-content: center; */
            align-items: center;
        }
        .child {
          width: 200px;
          height: 50px;
          background-color: aqua;
        }
```



##### flex为什么没有justify-items,justify-self

justify-items 不换行情况下只有一条主轴而又多个叫交叉轴，**一个 flex item 在主轴上的位置会影响其他 flex item**

justify-self不符合flex的设计，flex希望统一提供一种共同的主轴对其方式，以及影响其他项目

#### flex(flex-grow,flex-shrink,flex-basis)

默认值 0 1 auto

弹性布局的核心，如果项目总宽度(高度)超过容器的宽度（高度），取决与主轴方向

可以选择

- 换行
- 自动缩放

将超过的长度，按照每个项目的flex-shrink值比例缩小，三个200px宽,flex-shrink:1的元素在300px宽的项目中，超过长度=600-300=300，按照比例1+1+1=3，每个元素宽度缩小300/3=100，每个元素宽度为100px

flex-basis 默认值auto，及等于width值，设置之后可以代替width值，设置0%及完全由grow计算宽度



