# 从无到有做canvas



## 一、先有

随便先搞一个，出错了再改

```js
canvas.onclick = (e) => {
    console.log(e)
}
```

监听canvas的click事件，取到e，查看其可用属性，找到自己要用的属性，使用console.log进行调试

## 二、画点

取到点击位置，实现画点

```js
let div = document.createElement('div')
div.style.position = 'absolute'
div.style.left = e.clientX + 'px'   //clientX和Y是在上一步试出来的
div.style.top = e.clientY + 'px'
div.style.width = '6px'
div.style.height = '6px'
div.style.marginLeft = '-3px'
div.style.marginTop = '-3px'
div.style.borderRadius = '50%'
div.style.backgroundColor = 'black'
canvas.appendChild(div)
```

当鼠标点击时，在鼠标位置创建一个宽高6px的实心小黑圆，鼠标箭头出现在小圆的圆心，把div从内存渲染到页面

## 三、移动

在鼠标移动时画点，

将`onclick`替换为` onmousmove `

## 四、优化

JS操作DOM是跨线程，会很慢，所以用`canvas`代替`div`

`canvas`默认是`inline`元素，会有滚动条

所以

```css
display:block;
```



抄文档例子，改动数据，查看变化

```js
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

ctx.fillStyle = 'green';
ctx.fillRect(10, 10, 150, 100);
```



`canvas`类似`img`，可以设置初始宽高，但是会被css覆盖，css设置宽高会在最初设置的宽高基础上进行拉伸缩放

所以在最初设置`canvas`宽高时就使用JS获取屏幕的宽高，进行设置

获取到的屏幕宽高是`body`的宽高，`body`的高度是根据内容自适应的，所以获取的应该是`document`文档的宽高

```js
let canvas = document.getElementById("canvas")
canvas.width = document.documentElement.clientWidth
canvas.height = document.documentElement.clientHeight

let ctx = canvas.getContext("2d")

ctx.fillStyle = "blue"

canvas.onmousemove = (e) => {
    console.log(e.clientX)
    console.log(e.clientY)
    ctx.fillRect(e.clientX - 5,e.clientY - 5,10,10)
}
```



## 五、跟随鼠标

鼠标按下开始画线，鼠标松开抬起

初始事件是不画线，当鼠标按下画线,松开不画线

```js
let painting = false

canvas.onmousedown = () => {
    painting = true
}
canvas.onmouseup = () => {
    painting = false
}
```



## 六、消除锯齿

方形变圆形

抄文档

```js
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.beginPath();
ctx.arc(75, 75, 50, 0, 2 * Math.PI);
ctx.stroke();
```



## 七、连线

抄文档，调试

```js
function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext){
  var ctx = canvas.getContext('2d');

  // 填充三角形
  ctx.beginPath();
  ctx.moveTo(25, 25);
  ctx.lineTo(105, 25);
  ctx.lineTo(25, 105);
  ctx.fill();
```





## 八、适应手机

检测是否触屏设备 JS detect touch support

  ```js
let isTouchDevice = 'ontouchstart' in document.documentElement
  ```



## 九、成品

[canvas]()