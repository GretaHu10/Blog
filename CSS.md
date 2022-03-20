# 基础知识


> CSS真的是琐碎又麻烦，互相关联，互相影响，牵一发而动全身，一不小心全军覆没！！
> 但是它可以让你的页面条理又好看，在这个颜值当道的时代真的是神器！！
> 这篇博客是一些条理又常用的CSS知识！！加油！


## 一、CSS历史

1. 出现：李爵士的同事赖先生提出CSS的概念——层叠样式表：**样式层叠、选择器层叠、文件层叠**，这使得CSS极度灵活，但同时有些不受控制
2. 版本：
     * 使用最广泛：CSS 2.1，一直在发表，一直在更新，2004~2011
     * 现代版本：CSS 3，1999（此后的CSS都是分模块升级）




## 二、基础知识
### 1. 语法

* 样式语法

  ```css
     选择器{
         属性名:属性值;
         /*注释*/
     }   
  ```

  > 注：
  >
  > 英文符号；
  >
  > 区分大小写；
  >
  > 写错浏览器不会报错只会忽略



* at语法

  ```css
         @charset "UTF-8";           /*字符编码*/
   	   @import url(2.css);         /*导入一个额外的CSS文件*/
         @media (min-width:100px) and (max-width:200px){
             语法一
         }/*媒体查询*/
  ```

  


### 2. 方法
1）调试

* border调试大法   border占用空间时可以换成outline

* 验证器[W3C CSS validator](https://jigsaw.w3.org/css-validator/)

      

2）资料（搜索关键字后加）

   * MDN
   * CSS tricks
   * 张鑫旭
   * 浏览器兼容：[caniuse](https://caniuse.com/)
   * [css文档](https://www.w3.org/Style/CSS/specs.en.html)超长文档

3）标准

* W3C
* CSS spec
*  CSS 2.1 中文版





## 三、一些基本概念
### 1. 文档流(Normal Flow)

#### 1）概念

文档中内容正常的流动方向

* 从左到右：内联元素
* 从上到下：块级元素



#### 2）内联、块、内联块

|                  | 流动方向                                       | 宽度                                              | 高度                                    |
| ---------------- | ---------------------------------------------- | ------------------------------------------------- | --------------------------------------- |
| **inline**       | 从左到右，占满一行之后换行（会把自己分成两行） | 宽度为内部inline元素的和，尽量窄，不接受width指定 | 高度由line-height间接确定，不接受height |
| **block**        | 从上到下，每个都会另起一行                     | 默认自动计算宽度，尽量宽，是auto不是100%          | 由内部文档流元素决定，可以设height      |
| **inline-block** | 从左到右（不会把自己拆开）                     | 结合前两者特点，可用width，尽量窄                 | 与block相似                             |

* 
   任何元素都可以通过display属性来定义inline、block或者是inline-block

   


#### 3） 溢出 overflow
当内容超出容器就会溢出

```css
/*属性*/
overflow:visible;   /*默认，溢出就溢出*/

overflow:hidden;    /*溢出部分隐藏*/

overflow:scroll;    /*滚动条*/

overflow:auto;      /*需要的时候显示滚动条*/
```

* 如果有滚动条，内联元素默认在第一屏显示，后面留空

* `overflow`可以分为`overflow-x`和`overflow-y`

  

#### 4） 脱离文档流
* float
* position:absolute/fiexed





### 2. 盒模型 
#### 两种盒模型：

   * content-box 内容盒
        * `box-sizing:content-box;`
        * 内容就是盒子的边界 ，只包含content
   * border-box  边框盒
        * `box-sizing:border-box;`
        * Border才是盒子的边界，包含content、padding、border

![1645584707286](E:%5CBlog%5CCSS.assets%5C1645584707286.png)

* 举个栗子:两个都设置100px
```
 border:content+padding+border=100px；
 content:content(100px)+padding+border>100px
```


#### margin合并

```html
<!-- 代码示例-->
<head>
	<style>
        body{
            border:1px solid black;
        }
        .parent{
            margin:15px 0;
            border:1px solid yellow;
            background:red;
        }
        .child{
            margin:15px 0;
            border:1px solid green;
        }
    </style>
</head>
<body>
    <div class="parent">
        <div class="child">1</div>
        <div class="child">2</div>
        <div class="child">3</div>
    </div>
</body>
```

![1645843810465](E:%5CBlog%5CCSS.assets%5C1645843810465.png)



删除`parent`的`border` :



![1645844371424](E:%5CBlog%5CCSS.assets%5C1645844371424.png)

![1645844402568](E:%5CBlog%5CCSS.assets%5C1645844402568.png)

![1645844710023](E:%5CBlog%5CCSS.assets%5C1645844710023.png)

![1645844329845](E:%5CBlog%5CCSS.assets%5C1645844329845.png)

(1) 父子合并

`parent`的`margin-top`会和`child`1的`margin-top`合并

`parent`的`margin-bottom`会和`child`3的`margin-bottom`合并



(2) 兄弟合并

`child`1的`margin-bottom`和`child`2的`margin-top`合并，

`child`2的`margin-bottom`和`child`3的`margin-top`合并



(3)上下合并（左右不合并）

父子的`margin`设为上下左右都15px

![1645845406433](E:%5CBlog%5CCSS.assets%5C1645845406433.png)



(4)取消合并

* 加border

* 加padding

* overflow:hidden;

* display:flex;

* display:inline-block;

  
### 3. 基本单位（常用）
* 长度：
     * px 像素
     
     * em 在 font-size中使用是相对于父元素的字体大小，在其他属性中使用是相对于自身的字体大小，如 width=3*20
     
       ![1645845632523](E:%5CBlog%5CCSS.assets%5C1645845632523.png)
     
     * rem 根元素的字体大小
     
     * vm  vh
     
     * 百分数，整数
* 颜色：
     * 十六进制: #FFFFFF/#FFF，接受最后加透明度，取值是十六进制数#FF660088,50%的透明度
     * RGBA :rgb(0,0,0)/rgba(0,0,0,1) a是透明度，取值0~1
     * 色相hsl:(360,100%,100%)三个值分别是0~360彩虹色，饱和度，亮度，接受最后加透明度(360,100%,80%,0.5)



### CSS基础实践：小彩虹




# CSS布局
布局：把页面分成一块一块的三种布局方式：
* 固定宽度布局，一般为960/1000/1024px
* 不固定宽度布局，主要靠文档流自适应原理布局
* 响应式布局，PC上固定，手机上不固定——混合布局





## 一、Float布局
### 1.语法

兼容IE9，要给父元素添加.clearfix，必要时采用负margin

float布局是一种浮动，会使其元素脱离文档流，需要**清除浮动**:

在其父元素上添加.clearfix

```css
.clearfix::after{
    content:'';
    display:block;
    clear:both;
}
```

### 2.float实践

仿网站主页布局：Float

> 实践知识点：
>
> 1. 最后一栏宽度自适应
> 2. 在`padding:0;mairgin:0;`后，列表的黑点就看不到了，只是跑到了左侧，并不是没了，仍然需要reset
> 3.  给图片添加背景色等，会有超出边框的情况，解决办法：`vertical-align: top;`top可换成middle
> 4. 整块内容居中可使用`margin-left: auto;margin-right: auto;`两句，不使用`margin:0 auto;`,一句会覆盖top以及bottom
> 5. 平均布局内容宽度以及margin都需要计算，多出来的部分可以在父子之间添加一层`.x {margin-right: -12px;}`，采用**负margin**
> 6. 调试时border会占用宽度，可以换成outline





## 二、Flex布局

Flex有两种容器：

![1647484235377](E:%5CBlog%5CCSS.assets%5C1647484235377.png)



### 1.容器（父元素）：container

#### *1)让一个元素变成flex容器

```css
.container{
    display:flex;  *         /* 块弹性盒*/
    display:inline-flex;     /* 行内弹性盒*/  
}
```



#### *2)主轴items流动方向：

```css
.container{
    flex-direction:row; *            /*从左往右排列 →  */
    flex-direction:row-reverse;      /*从右往左排列 ←  */
    flex-direction:column; *         /*从上往下排列 ↓  */
    flex-direction:column-reverse;   /*从下往上排列 ↑  */
}
```

![1647484380602](E:%5CBlog%5CCSS.assets%5C1647484380602.png)



#### *3)折行：

不折行的话一行内的items会无限压缩自己

```css
.container{
    flex-wrap:wrap;  *       /*可折行，但不会拆item */
    flex-wrap:nowrap;        /*不折行  */ 
}
```

![1647484445286](E:%5CBlog%5CCSS.assets%5C1647484445286.png)



> flex-flow：
>
>    规定了flex-direction和flex-wrap 的缩写
>
> `flex-flow: [flex-direction] [flex-wrap] `



#### *4)主轴items摆放位置：

默认主轴为横轴，就是左右空间横向摆放位置

```css
.container{
    justify-content:center;    *      
    /* items挤在居中位置 */
    justify-content:flex-start;       
    /* items挤在开始位置 （如果主轴流动方向为→，则挤在左侧）*/
    justify-content:flex-end;         
    /* items挤在最后位置 （挤在右侧）*/
    justify-content:space-between; *  
    /* space分布在items之间 */
    justify-content:space-around;     
    /* space围绕在items周围 */
    justify-content:space-evenly;     
    /*space等分在items周围 */        
}
```

![1647485054860](E:%5CBlog%5CCSS.assets%5C1647485054860.png)





#### *5)次轴items摆放位置：

默认次轴为纵轴，就是上下空间纵向摆放位置

```css
.container{
    align-items:center;   *      
    /*高度不一的items中线对齐*/
    align-items:flex-start;      
    /*items顶部对齐*/
    align-items:flex-end;        
    /*items底部对齐*/
    align-items:stretch;         
    /*默认项，不一样的items拉长等高*/
    align-items:baseline;     
}
```

![1647485378701](E:%5CBlog%5CCSS.assets%5C1647485378701.png)





#### 6）多行items摆放分布：

```css
.container{
    align-content:center;
    align-content:flex-start;
    align-content:flex-end;
    align-content:stretch;
    align-content:space-between; 
    align-content:space-around;  
}
```

![1647485437043](E:%5CBlog%5CCSS.assets%5C1647485437043.png)







###   2.项目（子元素）：items

#### 1)改变item顺序：order

```css
.item:first-child{
    order:4;
}/*第一个item排在第四位*/
.item:nth-child(2){
    order:1;
}/*第二个item排在第一位*/
.item:nth-child(3){
    order:3;
}/*第三个item排在第三位*/
.item:last-child{
    order:2;
}/*最后一个item排在第二位*/
```



![1647484333966](E:%5CBlog%5CCSS.assets%5C1647484333966.png)



#### 2) 分配多余空间（吃胖）：flex-grow

（不加就每个item能有多窄挤到多窄）

* 加到父item上就是平均分配
 `.item{ flex-grow:1; }`
* 选择在每个child item上加n份
```css
.item:first-child{
    flex-grow:0;
}
.item:nth-child(2){
    flex-grow:1;
}
.item:nth-child(3){
    flex-grow:0;
}/*把多余的空间都给第二个child item*/
```

![1647485491097](E:%5CBlog%5CCSS.assets%5C1647485491097.png)







#### 3)压缩不够的空间（挤瘦）：flex-shrink

item固定宽度，当空间不够时，压缩谁的空间，不加就每个item同步缩

```css
.item:first-child{
    flex-shrink:1;
}
.item:nth-child(2){
    flex-shrink:50;
}
.item:nth-child(3){
    flex-shrink:1;
}/*当空间被压缩到不够时，2贡献50份被压缩空间，一和三各贡献一份*/
```
`flex-shrink:0;`表示我不要瘦



#### 4)控制基准宽度：flex-basis

默认值为auto



#### 5）定制align-items

align-self可定制align-items，控制某一个item是上对齐还是下对齐


  ```css
  .item:first-child{
      align-self:flex-start;/*当前item顶头上对齐*/
      align-self:flex-end;/*当前item沉底下对齐*/
  }
  ```

 ![1647485578770](E:%5CBlog%5CCSS.assets%5C1647485578770.png)



> `*`表示常用



### 3. Flex实践

小游戏：[小青蛙](http://flexboxfroggy.com/)

仿网站主页布局：Flex

> 实践知识点：
>
> div想要使用`justify-content:center;`来达到在页面中居中的效果，需给它一个父元素，在父元素中使用该语句使其达到居中，一般使用`margin-right:auto;margin-left:auto;`来使一整块内容居中



**内容来源**：[Flexbox |的完整指南CSS-技巧 - CSS-技巧 (css-tricks.com)](https://css-tricks.com/snippets/css/a-guide-to-flexbox/#aa-flex-flow)





## 三、Grid布局

Grid布局适用于各种不规则布局，较前两种布局灵活，属性很多，还有各种缩写，这里仅列几种常见用法。

### 1. 容器（父元素）：container

![1647500269453](E:%5CBlog%5CCSS.assets%5C1647500269453.png)



### 2. 表格布局

#### 1) 设置行高及列宽


```css
.container{
    grid-template-columns: 40px 50px auto 50px 40px;  
    /*分为几列以及列宽*/
    grid-template-rows:25% 100px auto;  
    /*分为几行以及行高 */
}
```
![1647500348040](E:%5CBlog%5CCSS.assets%5C1647500348040.png)



#### 2)fr:free space  

份，把每行每列设置占几份

```css
.container{
    grid-template-columns: 1fr 2fr 1fr; 
    /*分为几列以及列宽*/
    grid-template-rows:1fr 1fr;  
    /*平均分为2行  */
}
```



#### 3)表格线（定制item）

可以给每一条表格线起名字，默认为数字，比如想要控制第一个单元格占到第一行的前两列

```css
.item:first-child{
   grid-row-start:1;
   grid-row-end:2;
    /*竖着方向从第一条线开始，到第二条线结束*/
   grid-column-start:1;
   grid-column-end:3;
    /*横着方向从第一条线开始，到第三条线结束*/
}
```


![1647500456225](E:%5CBlog%5CCSS.assets%5C1647500456225.png)



#### 4）分区 grid-template-areas


```html
<div class="container">
  <header>h</header>
  <main>m</main>
  <aside>ad</aside>
  <footer>f</footer>
</div>
```

```css
.container {
  border:1px solid black;
  display: inline-grid;
  grid-template-columns: 50px 50px 50px 50px;
  grid-template-rows: 50px 50px 50px;
  grid-template-areas: 
    "header header header header"
    "main main . sidebar"
    "footer footer footer footer";
}/*“.”一般表示不要占据这个地方*/

.container>header {
  grid-area: header;
  border:1px solid red;
}/*header标签占areas里的header地方*/

.container>main {
  grid-area: main;
  border:1px solid red;
}

.container>aside {
  grid-area: sidebar;
  border:1px solid red;
}

.container>footer {
  grid-area: footer;
  border:1px solid red;
}
```

![1647502659039](E:%5CBlog%5CCSS.assets%5C1647502659039.png)



#### 5）空隙 gap 

```css
.container{
    grid-gap:10px;     /*所有空隙都是10px*/
    column-gap:10px;   /*列间空隙10px*/
    row-gap:15px;      /*行间空隙15px*/
}
```

 ![1647502970585](E:%5CBlog%5CCSS.assets%5C1647502970585.png)



### 3. Grid实践

小游戏：[小花园](http://cssgridgarden.com/)

内容来源：[A Complete Guide to Grid | CSS-Tricks - CSS-Tricks](https://css-tricks.com/snippets/css/complete-guide-grid/)





# CSS定位

在屏幕看来，我们所做的网页都是一个平面，但是它其实是有上下关系的，就像是千层蛋糕一样。CSS定位更符合于CSS层叠样式表这个概念，而定位就是在告诉这个蛋糕奶油要放到哪一层，水果要放到哪一层。

**定位用到的只有一个属性`position`**

## 一、一个div的分层
可以通过一些简单的代码（添加半透明颜色）测试一下他们的分层关系，从上到下依次是：

  * ……
  * 定位元素z-index：2
  * 定位元素z-index：1
  * 定位元素z-index：0
  * 内联元素
  * 浮动元素
  * 块级子元素
  * border
  * background
  * 定位元素z-index：-1
  * 定位元素z-index：-2
  * ……



## 二、position的五个取值

* static  默认值，就在文档流里好好呆着

* relative  相对定位，升起来，但不脱离文档流，占的位置还是原位，显示时偏移

* absolute  绝对定位，定位基准是祖先里的非static

* fixed   固定定位，定位基准是viewport，有坑

* sticky  粘滞定位，不常用，适用于导航



### 1.position:relative

####  1)使用场景

   * 用于做位移
   * absolute元素的父元素



#### 2)配合z-index

   * z-index:auto;默认值，相当于0，但不是0，不创建新的层叠上下文
   * z-index:0/1/2;
   * z-index:-1/-2;



### 2. position:absolute

#### 1)使用场景

   * 脱离原来的位置，另起一层，比如对话框的关闭按钮
   * 鼠标提示，设置一个按钮，鼠标浮上显示提示内容
   ```html
   <button>点击
        <span class=tips>提示内容</span>
   </button>
   ```
   ```css
   button{
       position:relative;
   }
   button span{
       position:absolute;
       /*必须在定位上一级有“position:relative;”*/
       border:1px solid red;
       white-space:nowrap;
       /*文字内容不准换行*/
       bottom:calc(100% + 10px);
       /*距离底部100%再加10px，飘在按钮上方10px*/
       left:50%;
       transform:translateX(-50%);
       /*X轴位移*/
       display:none;
   }
   button:hover span{
       display:inline-block;
   }/*鼠标浮上显示*/
   ```



#### 2)配合z-index

#### 3)注意：

* 相对于祖先元素最近的一个position且不是static的元素进行定位
* top/left不能省略
* left: 100%; left: 50%;  加负margin



### 3.position:fixed

#### 1)使用场景（相对于视口定位）

   * 广告
   * 回顶部按钮 

会和transform属性冲突



#### 2)配合z-index



## 三、层叠上下文

* 每一层都是一个小世界
* 这个小世界里的z-index与外界无关
* 处在同一个小世界的z-index才能比较
  





# CSS动画
 动画是许多静止的画面（帧）连续播放
 影视剧一般每秒24帧，游戏最低30帧，静态动画每秒15帧



## 一、 浏览器渲染原理

### 1.渲染过程

 * 根据HTML构建HTML树（DOM）
 * 根据CSS构建CSS树（CSSOM）
 * 将两棵树合并成一棵渲染树（render tree）
 * Layout布局（文档流、盒模型、计算大小和位置）
 * Paint绘制（边框颜色、文字颜色、阴影等）
 * Composite合成（根据层叠关系展示画面）



### 2.使用JS更新样式时的三种方式

|      | JS/CSS | Style | Layout | Paint | Composite | 应用          |
| ---- | ------ | ----- | ------ | ----- | --------- | ------------- |
| 1    | →      | →     | →      | →     | →         | div.remove()  |
| 2    | →      | →     |        | →     | →         | 改变背景颜色  |
| 3    | →      | →     |        |       | →         | 改变transform |



### 3.CSS动画优化

优化过程对应各个渲染过程

#### 1）JS优化

- 对于动画效果的实现，**避免使用 setTimeout 或 setInterval，请使用 requestAnimationFrame**。
- 将长时间运行的 JavaScript 从主线程移到 Web Worker。
- 使用微任务来执行对多个帧的 DOM 更改。
- 使用 Chrome DevTools 的 Timeline 和 JavaScript 分析器来评估 JavaScript 的影响。



#### 2）Style优化

- 降低选择器的复杂性；使用以类为中心的方法，例如 BEM。
- 减少必须计算其样式的元素数量。



#### 3）布局优化

- 布局的作用范围一般为整个文档。
- DOM 元素的数量将影响性能；应尽可能避免触发布局。
- 评估布局模型的性能；新版 Flexbox 一般比旧版 Flexbox 或基于浮动的布局模型更快。
- 避免强制同步布局和布局抖动；先读取样式值，然后进行样式更改。



#### 4）绘制优化

- 除 transform 或 opacity 属性之外，更改任何属性始终都会触发绘制。
- 绘制通常是像素管道中开销最大的部分；应尽可能避免绘制。
- 通过层的提升和动画的编排来减少绘制区域。
- 使用 Chrome DevTools 绘制分析器来评估绘制的复杂性和开销；应尽可能降低复杂性并减少开销。



#### 5）合成优化

- 坚持使用 **transform **和 opacity 属性更改来实现动画。
- 使用 **will-change**或 `translateZ` 提升移动的元素。
- 避免过度使用提升规则；各层都需要内存和管理开销。



[渲染性能  |  Web  |  Google Developers](https://developers.google.com/web/fundamentals/performance/rendering)

 CSS Triggers可查看每个属性触发什么流程 




##  二、 transform
一般都需要配合transition过渡，inline元素不支持transform
### 1.位移translate

```css
transform:translateX(<length-prcentage>); 
/*X轴  <length-prcentage>长度或者百分数*/

transform:translateY(<length-prcentage>); 
/*Y轴*/

transform:translate(<length-prcentage>,<length-prcentage>); 
/*X轴,Y轴*/

transform:translateZ(<length-prcentage>); 
/*Z轴需在父元素添加视点perspective，视觉效果为近大远小*/

transform:translate3d(<length-prcentage><length-prcentage>,<length-prcentage>);
```





### 2.缩放scale

```css
transform:scaleX(<number>);
/*横向长肉*/

transform:scaleY(<number>);
/*纵向长高*/

transform:scale(<number>,<number>);
```

> scale使用较少，会变形，border也会跟着缩放



### 3.旋转rotate

```css
trabsform:rotate([<angle>|<zero>]);
trabsform:rotateX([<angle>|<zero>]);
trabsform:rotateY([<angle>|<zero>]);
trabsform:rotateZ([<angle>|<zero>]);
```

> 应用：加载旋转动画  deg度数单位



### 4.倾斜skew

```css
transrorm:skewX([<angle>|<zero>]);
transrorm:skewY([<angle>|<zero>]);
transrorm:skew([<angle>|<zero>],[<angle>|<zero>]);
```





### 5.其他用法

```css
/*多重效果组合使用*/
transform:scale(0.5) translate(-100%,-100%);

/*取消所有*/
transform:none;

/*绝对居中*/
.parent{
    position:relative;
}
.children{
    position:absolute;
    left:50%;
    top:50%;
    transform:translate(-50%,-50%);/*在X轴Y轴回半个身位*/
}
```



[transform - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform)





##  三、 transition
transition过渡，补充中间帧，放在父类里

### 1. 语法格式

```css
transition:属性名 时长 过渡方式 延迟;
```

> **transition** CSS属性是 `transition-property`，`transition-duration`,`transition-timing-function`和 `transition-delay`的一个简写属性。

* **属性名**：width|height|all|left|top等，可以用逗号分隔两个不同属性
* **时长**：用秒s或者毫秒ms
* **过渡方式**：linear|ease|ease-in|ease-out|ease-in-out|cubic-bezier|step-start-end|steps
* **延迟**用秒s或毫秒ms



### 2.两次过渡

通过中间点进行过渡，a=>b=>c，c继承b的状态之后进行二次过渡，可以通过setTimeout或者监听transitionend事件



### 3.不是所有属性都可以过渡

**消失无法过渡**

* `display : none => block; `  无法过渡

* 使元素消失可使用：

  * `opacity:1;→opacity:0;`使其变透明，

  * `visibity:hidden;→visible`使其看不见，能见度

以上两种方法都是使其看不见，但位置还在。

可添加JS代码控制一段时间后删除`setTimeout(() = > {demo.remove()},1000)`透明度



> 有规律就是可靠的过渡
>
> 过渡有始有终，一般一次动画，或者两次



[transition - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition)



## 四、animation

### 语法格式：

#### 1）animation

```css
animation:动画名 时长 过渡方式 延迟 次数 方向 填充模式 是否暂停;
```

> **animation** CSS属性是 `animation-name`，`animation-duration`, `animation-timing-function`，`animation-delay`，`animation-iteration-count`，`animation-direction`，`animation-fill-mode`和 `animation-play-state`属性的一个简写属性形式。

- **次数**：3/2.4、infinite无限次

- **方向**：normal | reverse | alternate | alternate-reverse

- **填充模式**：none | forwards | backwards | both

- **是否暂停**：running | paused

  

**forwards**让动画停在最后一帧



[animation - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation)



#### 2）@keyframes

```css
@keyframes slidein {
  from {
    transform: translateX(0%); 
  }

  to {
    transform: translateX(100%);
  }
}
```



```css
@keyframes identifier {
  0% { top: 0; left: 0; }
  30% { top: 50px; }
  68%, 72% { left: 50px; }
  100% { top: 100px; left: 100%; }
}

```

[@keyframes - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@keyframes)




### 五、动画实践

1. [transform配合transition过渡，添加hover制作跳动的红心](http://js.jirengu.com/quwodeluze/2/edit?html,css,output)
2. [animation跳动的心](http://js.jirengu.com/weyaxeraba/1/edit?html,css,output)







<hr/>




display:
display属性有两个作用，一是定义元素的类型（块级元素或行内元素），规定元素的流式布局；二是控制其子元素的布局（grid或flex）

div无内容高度为0，span无内容宽度为0

container:容器，集装箱

item:项目，列表里的每一项

direction:方向

wrap:包裹

justify：证明合法；整理版面

content：内容，目录；容量；满足

align：排列，使成一行；匹配

white-space:nowrap;文字内容不换行

opacity也是透明度，会影响整个div内所有元素

border-radius: 50% 50% 50% 50%;控制矩形圆角 radius半径

transform：变换，改变

visibility:能见度



```css
/* property: calc(expression)*/
width: calc(100% - 80px);
```
此 calc()函数用一个表达式作为它的参数，用这个表达式的结果作为值。这个表达式可以是任何如下操作符的组合，采用标准操作符处理法则的简单表达式。<br/>