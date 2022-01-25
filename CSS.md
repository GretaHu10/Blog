# 基础知识


> CSS真的是琐碎又麻烦，互相关联，互相影响，牵一发而动全身，一不小心全军覆没！！
> 但是它可以让你的页面条理又好看，在这个颜值当道的时代真的是神器！！
> 这篇博客是一些条理又常用的CSS知识！！加油！


## 一、CSS历史

1. 出现：李爵士的同事赖先生提出CSS的概念——层叠样式表：**样式层叠、选择器层叠、文件层叠**，这使得CSS极度灵活，但同时有些不受控制

2. 版本：
     * 使用最广泛：CSS 2.1，一直在发表，一直在更新，2004~2011
     * 现代版本：CSS 3，分模块，1999（此后的CSS都是分模块升级）

3. 浏览器兼容：[caniuse](https://caniuse.com/)

4. 请感性的活在CSS的世界中




## 二、基础知识
1. 语法
     * 样式语法
       ```css
           选择器{
               属性名:属性值;
               /*注释*/
           }
       ```
       英文符号；
       区分大小写；
       写错浏览器不会报错只会忽略
       
     * at语法
       ```css
           @charset "UTF-8";  /*字符编码*/
           @import url(2.css);/*导入一个额外的CSS文件*/
           @media (min-width:100px) and (max-width:200px){
               语法一
           }/*媒体查询*/
       ```

2. 调试
     * border调试大法
3. 资料（搜索关键字后加）
     * MDN
     * CSS tricks
     * 张鑫旭
4. 标准
     * W3C
     * CSS spec
     * CSS 2.1 中文版

## 三、一些基本概念
1. 文档流(normal flow)
     * 从左到右：内联元素
     * 从上到下：块级元素
2. 内联、块、内联块<br/>
   (1) 流动方向<br/>
     * inline：从左到右，占满一行之后换行（会把自己分成两行）<br/>
     * block：从上到下，每个都会另起一行
     * inline—block：从左到右（不会把自己拆开）<br/>
   任何元素都可以通过display属性来定义inline、block或者是inline-block<br/>

   (2) 宽度<br/>

     * inline：宽度为内部inline元素的和，尽量窄，不接受width指定
     * block：默认自动计算宽度，尽量宽，是auto不是100%
     * inline—block：结合前两者特点，可用width，尽量窄<br/>

   (3) 高度<br/>
     * inline：高度由line-height间接确定，不接受height
     * block：由内部文档流元素决定，可以设height
     * inline—block：与block相似<br/>

3. 溢出 overflow
当内容超出容器就会溢出
```
overflow:visible;  #默认，溢出就溢出
```
```
overflow:hidden;  #溢出的不给你看，隐藏
```
```
overflow:scroll;  #滚动条
```
```
overflow:auto;  #需要的时候显示滚动条
```
* 如果有滚动条，内联元素默认在第一屏显示，后面留空
* overflow可以分为overflow-x和overflow-y
4. 脱离文档流
* float
* position:absolute/fiexed

5. 盒模型<br/>
   两种盒模型：
     * content-box 内容盒，内容就是盒子的边界 `box-sizing:content-box;`
     * border-box  边框盒，边框才是盒子的边界`box-sizing:border-box;`

![](https://user-gold-cdn.xitu.io/2020/3/22/171010d19801cb1e?w=1548&h=575&f=jpeg&s=140617)
* content-box只包含content
* border-box包括content、padding、border
* 举个栗子:两个都设置100px
```
 border:content+padding+border=100px；
 content:content(100px)+padding+border>100px
```
6. margin合并<br/>
(1)父子合并<br/>

![](https://user-gold-cdn.xitu.io/2020/3/22/1710127f5f24b70c?w=1230&h=364&f=png&s=216407)

![](https://user-gold-cdn.xitu.io/2020/3/22/171012871b8ef819?w=410&h=344&f=png&s=51828 )
删除parent的border，parent的高度会变矮<br/>
(2)兄弟合并<br/>

![](https://user-gold-cdn.xitu.io/2020/3/22/171012408ed15f72?w=1227&h=227&f=png&s=152723)
`display:inline-block;`不会合并<br/>
(3)上下合并（左右不合并）<br/>

(4)取消合并
* 加border
* 加padding
* overflow:hidden;
* display:flex;
<br/>
7. 基本单位（常用）
* 长度：
     * px 像素
     * em 在 font-size中使用是相对于父元素的字体大小，在其他属性中使用是相对于自身的字体大小，如 width
     * rem 根元素的字体大小
* 颜色：
     * 十六进制: #FFFFFF/#FFF
     * RGBA :rgb(0,0,0)/rgba(0,0,0,1) a是透明度
     * 色相hsl:(360,100%,100%)三个值分别是0~360彩虹色，饱和度，亮度
    <br/><br/><br/><br/><br/><br/>


# CSS布局
布局：把页面分成一块一块的<br/>
三种布局方式：
* 固定宽度布局，一般为960/1000/1024px
* 不固定宽度布局，主要靠文档流自适应原理布局
* 响应式布局，PC上固定，手机上不固定——混合布局

## 一、Float布局
兼容IE9，要给父元素添加.clearfix，必要时采用负margin<br/>
float布局是一种浮动，会使其元素脱离文档流，需要**清除浮动**:<br/>
在其父元素上添加.clearfix
```css
.clearfix::after{
    content:'';
    display:block;
    clear:both;
}
```

## 二、Flex布局
1. 容器（父元素）：container
* display:flex|inline-flex;

  ```css
  .container{
      display:flex;          /* 块弹性盒*/
      display:inline-flex;   /* 行内弹性盒*/  
  }
  ```
  
* 主轴流动方向：flex-direction:row|row-reverse|column|column-reverse;

  ```css
  .container{
      flex-direction:row;           /*从左往右排列 →  */
      flex-direction:row-reverse;   /*从右往左排列 ←  */
      flex-direction:column;        /*从上往下排列 ↓  */
      flex-direction:column-reverse;/*从下往上排列 ↑  */
  }
  ```
  
* 折行：flex-wrap:wrap|nowrap;
不折行的话一行内的items会无限压缩自己

  ```css
  .container{
      flex-wrap:wrap;       /*可折行，但不会拆item */
      flex-wrap:nowrap;     /*不折行  */  
  }
  ```

* 主轴items摆放位置：justify-content:center|flex-start|flex-end|space-between|space-around|space-evenly;（默认主轴为横轴，就是左右空间横向摆放位置）

  ```css
  .container{
      justify-content:center;/* items挤在居中位置 */
      justify-content:flex-start;/* items挤在开始位置 （如果主轴流动方向为→，则挤在左侧）*/
      justify-content:flex-end;/* items挤在最后位置 （挤在右侧）*/
      justify-content:space-between;/* space分布在items之间 */
      justify-content:space-around;/* space围绕在items周围 */
      justify-content:space-evenly;   /*space等分在items周围 */        
  }
  ```

![](https://user-gold-cdn.xitu.io/2020/3/31/17130d8915cc2e6d?w=342&h=557&f=png&s=181273)


* 次轴items摆放位置：align-items:center|flex-start|flex-end|stretch|baseline;（默认次轴为纵轴，就是上下空间纵向摆放位置）

  ```css
  .container{
      align-items:center;/*高度不一的items中线对齐*/
      align-items:flex-start;/*items顶部对齐*/
      align-items:flex-end;/*items底部对齐*/
      align-items:stretch;/*默认项，不一样的items拉长等高*/
      align-items:baseline;     
  }
  ```
  

![](https://user-gold-cdn.xitu.io/2020/4/8/1715811f7258c4c8?w=311&h=377&f=png&s=141105)


* 多行items摆放分布：align-content:center|flex-start|flex-end|stretch|space-between|space-around;


![](https://user-gold-cdn.xitu.io/2020/4/8/171592c19d41d3d5?w=309&h=408&f=png&s=172386)

  

2. 项目（子元素）：items
* 改变item顺序：order

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

* 分配多余空间（吃胖）：flex-grow（不加就每个item能有多窄挤到多窄）

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

* 压缩不够的空间（挤瘦）：flex-shrink（item固定宽度，当空间不够时，压缩谁的空间，不加就每个item同步缩）

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
* 控制基准宽度：flex-basis;
* align-self可定制align-items，控制某一个item是上对齐还是下对齐


  ```css
  .item:first-child{
      align-self:flex-start;/*当前item顶头上对齐*/
      align-self:flex-end;/*当前item沉底下对齐*/
  }
  ```


  CSS属性 flex 规定了弹性元素如何伸长或缩短以适应flex容器中的可用空间。这是一个简写属性，用来设置 flex-grow, flex-shrink 与 flex-basis

## 三、Grid布局

1. 容器（父元素）：container
* display:grid|inline-grid;
2. 表格布局
* 设置行高及列宽


```css
.container{
    grid-template-columns: 40px 50px auto 50px 40px;  /*分为几列以及列宽*/
    grid-template-rows:25% 100px auto;  /*分为几行以及行高 */
}
```
![](https://user-gold-cdn.xitu.io/2020/4/12/1716c5a8a8755928?w=634&h=472&f=png&s=113885)

* fr:free space  份，把每行每列设置占几份

```css
.container{
    grid-template-columns: 1fr 2fr 1fr; /*分为几列以及列宽*/
    grid-template-rows:1fr 1fr;  /*平均分为2行  */
}
```

* 可以给每一条表格线起名字，默认为数字，比如想要控制第一个单元格占到第一行的前两列

```css
.item:first-child{
   grid-row-start:1;
   grid-row-end:2;/*竖着方向从第一条线开始，到第二条线结束*/
   grid-column-start:1;
   grid-column-end:3;/*横着方向从第一条线开始，到第三条线结束*/
}
```
3. 分区 grid-template-areas


```html
<div class="container">
    <header>h</header>
    <aside>as</aside>
    <main>m</main>
    <div class="ad">ad</div>
    <footer>f</footer>
</div>
```

```css
.container{
    display:grid;
    grid-templete-columns:60px auto 60px;
    grid-templete-rows:100px auto 100px;
    grid-templete-areas:
     "header  header haeder"
     "aside main ad"
      ". footer footer"
}/*“.”一般表示不要占据这个地方*/
.container > header{
    grid-area: header;
}/*header标签占areas里的header地方*/
.container > aside{
    grid-area: aside;
}
.container > main{
    grid-area: main;
}
.container > .ad{
    grid-area: ad;
}
.container > footer{
    grid-area: footer;
}
```


4. 空隙 gap 

```css
.container{
    grid-gap:10px;/*所有空隙都是10px*/
    grid-columns-gap:10px;/*列间空隙10px*/
    grid-rows-gap:5px;/*行间空隙5px*/
}
```

 <br/><br/> <br/><br/> <br/><br/>
# CSS定位

在屏幕看来，我们所做的网页都是一个平面，但是它其实是有上下关系的，就像是千层蛋糕一样。CSS定位更符合于CSS层叠样式表这个概念，而定位就是在告诉这个蛋糕奶油要放到哪一层，水果要放到哪一层。
定位用到的只有一个属性position

1. 一个div的分层
可以通过一些简单的代码测试一下他们的分层关系，从上到下依次是：
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

2. position的五个取值
* static  默认值，就在文档流里好好呆着
* relative  相对定位，升起来，但不脱离文档流，占的位置还是原位，显示时偏移
* absolute  绝对定位，定位基准是祖先里的非static
* fixed   固定定位，定位基准是viewport，有坑
* sticky  粘滞定位，不常用，适用于导航<br/><br/>

<一>、position:relative<br/><br/>
1)使用场景
   * 用于做位移
   * 给absolute元素做爸爸 <br/><br/>

2)配合z-index
   * z-index:auto;默认值，相当于0，但不是0，不创建新的层叠上下文
   * z-index:0/1/2;
   * z-index:-1/-2;<br/><br/>
<br/><br/>

<二>、position:absolute<br/><br/>
1)使用场景
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
       position:absolute;/*必须在定位上一级有“position:relative;”*/
       border:1px solid red;
       white-space:nowrap;
       bottom:calc(100% + 10px);/*距离底部100%再加10px，飘在按钮上方10px*/
       left:50%;
       transform:translateX(-50%);/*X轴位移*/
       display:none;
   }
   button:hover span{
       display:inline-block;
   }/*鼠标浮上显示*/
   ```

<br/><br/>

2)配合z-index<br/><br/>
3)相对于祖先元素最近的一个position且不是static的元素进行定位
   <br/><br/><br/><br/>

<三>、position:fixed<br/><br/>
1)使用场景（相对于视口定位）
   * 广告
   * 回顶部按钮 <br/><br/>

2)配合z-index
 <br/><br/>

3. 层叠上下文
* 每一层都是一个小世界
* 这个小世界里的z-index与外界无关
* 处在同一个小世界的z-index才能比较
  <br/><br/> <br/><br/> <br/><br/>





# CSS动画
 动画是许多静止的画面（帧）连续播放
 影视剧一般每秒24帧，游戏最低30帧<br/><br/><br/><br/>
### 一、 浏览器渲染原理
 1. 渲染过程
 * 根据HTML构建HTML树（DOM）
 * 根据CSS构建CSS树（CSSOM）
 * 将两棵树合并成一棵渲染树（render tree）
 * Layout布局（文档流、盒模型、计算大小和位置）
 * Paint绘制（边框颜色、文字颜色、阴影等）
 * Composite合成（根据层叠关系展示画面）
 <br/><br/><br/><br/>
 2. 使用JS更新样式时的三种方式
 * `JS/CSS > 样式（Style）> 布局（Layout）> 绘制（Paint）> 合成（Composite）`例如`div.remove()`
 * `JS/CSS > Style > Paint > Composite`例如改变背景颜色
 * `JS/CSS > Style > Composite`例如改变transform <br/>

 CSS Triggers可查看每个属性触发什么流程
<br/><br/> <br/><br/> 


###  二、 transform
一般都需要配合transition过渡，inline元素不支持transform
1. 位移translate
* `translateX(<length-prcentage>)`X轴
* `translateY(<length-prcentage>)`Y轴
* `translate(<length-prcentage>,<length-prcentage>)`X轴，Y轴
* `translateZ(<length>)`Z轴需在父元素添加视点perspective，视觉效果为近大远小
* `translate3d(x,y,z)`
2. 缩放scale
* `scaleX(<number>)`横向长肉
* `scaleY(<number>)`纵向长高
* `scale(<number>,<number>)`
3. 旋转rotate
* `rotate([<angle>|<zero>])`
* `rotateZ([<angle>|<zero>])`
* `rotateX([<angle>|<zero>])`
* `rotateY([<angle>|<zero>])`
4. 倾斜skew
* `skewX([<angle>|<zero>])`
* `skewY([<angle>|<zero>])`
* `skew([<angle>|<zero>],[<angle>|<zero>]?)`
5. 多重效果组合使用

* `transform:scale(0.5) translate(-100%,-100%);`
* `transform:none;`取消所有





###  三、 transition
transition过渡，补充中间帧，放在父类里


1. 语法格式：transition:属性名 时长 过渡方式 延迟
* 属性名：width|height|all|left|top等，可以用逗号分隔两个不同属性
* 时长：用秒s或者毫秒ms
* 过渡方式：linear|ease|ease-in|ease-out|ease-in-out|cubic-bezier|step-start-end|steps
* 延迟用秒s或毫秒ms

2. 不是所有属性都可以过渡
* display:none<=>block   无法过渡
* 消失使用`opacity:1;→opacity:0;`使其变透明，可添加JS代码控制一段时间后删除`setTimeout(() = > {demo.remove()},1000)`透明度
* 也可使用`visibity:hidden;→visible`使其消失，但位置还在，能见度
* 有规律就是可靠的过渡
* 过渡有始有终，一般一次动画，或者两次


### 四、两次过渡——animation

1. 可使用两次transform


![](https://user-gold-cdn.xitu.io/2020/5/9/171f79edac9d17cd?w=1080&h=1394&f=jpeg&s=161498)

2. 使用animation


![](https://user-gold-cdn.xitu.io/2020/5/9/171f79f7ba38c18e?w=1080&h=2046&f=jpeg&s=245706)


![](https://user-gold-cdn.xitu.io/2020/5/9/171f79ffefcdebd7?w=1080&h=1715&f=jpeg&s=200142)



![](https://user-gold-cdn.xitu.io/2020/5/9/171f7a01ca0bcdc9?w=1080&h=1443&f=jpeg&s=195284)




### 五、动画实践

1. 使用JS更新样式
* 添加样式  
     * `div.style.background='red'`
     * `div.style.display='none'`
* 添加类 `div.classList.add('red')`
* 直接删除节点 `div.remove()`

2. [transform配合transition过渡，添加hover制作跳动的红心](http://js.jirengu.com/quwodeluze/2/edit?html,css,output)
3. [animation跳动的心](http://js.jirengu.com/weyaxeraba/1/edit?html,css,output)

<hr/>
display:
display属性有两个作用，一是定义元素的类型（块级元素或行内元素），规定元素的流式布局；二是控制其子元素的布局（grid或flex）<br/><br/>
div无内容高度为0，span无内容宽度为0<br/>
container:容器，集装箱<br/>
item:项目，利用表里的每一项<br/>
direction:方向<br/>
wrap:包裹<br/>
justify：证明合法；整理版面<br/>
content：内容，目录；容量；满足<br/>
align：排列，使成一行；匹配<br/>
white-space:nowrap;文字内容不换行<br/>
opacity也是透明度，会影响整个div内所有元素<br/>
border-radius: 50% 50% 50% 50%;控制矩形圆角 radius半径<br/>
transform：变换，改变<br/>
visibility:能见度<br/>

```css
/* property: calc(expression)*/
width: calc(100% - 80px);
```
此 calc()函数用一个表达式作为它的参数，用这个表达式的结果作为值。这个表达式可以是任何如下操作符的组合，采用标准操作符处理法则的简单表达式。<br/>
CSS Tricks