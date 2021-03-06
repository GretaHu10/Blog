# 一、对象定义

对象object唯一一种复杂类型



## 1.定义：

* 无序的数据集合

* 键值对的集合



## 2. 写法

```js 
let obj = {'name': 'frank','age':18}
//简写声明一个对象并赋值

let obj = new Object({'name':'frank'})
//完整写法，使用构造函数声明对象并赋值

console.log({'name': 'frank','age':18})
//打印输出一个对象
```

![1649855945906](JS对象.assets\1649855945906.png)



 ## 3. 规则

* 键名是**字符串**，不是标识符，可以包含任意字符，空字符串、中文、表情、空格等都可以
* 引号可以省略，省略之后就只能写标识符和数字
* 就算引号省略了，键名也还是字符串
* 数字也是字符串，没有数字键名和数字下标，都是字符串



## 4. 组成

### （1）属性名

#### 1）规则

* 每个key都是对象的属性名（property）

* 所有属性名会自动变成字符串



#### 2）特殊的key

* **数字**做属性名

  ```js
  let obj = {
      1:'a',
      3.2:'b',
      1e2:'c',
      .234:true,
      oxFF:true
  };
  Object.keys(obj)
  ["1","100","255","3.2","0.234","255"]
  ```

  > 想保留原有属性名就加引号



* **变量**做属性名

  ```js
  let p1 = 'name'
  
  let obj = {p1 :'frank'}   //属性名为‘p1’
  let obj = {[p1] : 'frank'}   //属性名为‘name’
  let obj = {['p1']:'frank'}    //属性名为‘p1’
  ```
```
  
  > 不加[]的属性名会自动变成字符串
  >
  > 加[]则会当做变量求值
  >
  > 值如果不是字符串，就会自动变成字符串
  >
  > `obj.p1===obj['p1']`

 

* **symbol**做属性名

​```js
let a = Symbol()
let obj = {[a]:'hello'}
```



### （2）属性值

每个value都是对象的属性值 



# 二、增删改查   对象的属性

## 1. 删

```js
//删除属性值
obj.xxx = undefined


//删除属性名
delete obj.xxx
delete obj['xxx']
//删除掉属性名会同时删除obj的xxx属性
```





## 2. 查（读）

### （1）查看自身属性

```js
//查看自身属性名
Object.keys(obj)

//查看自身属性值
Object.values(obj)

//查看自身属性名和属性值
Object.entries(obj)
```

> 以上都是以**数组**形式打印，这种方式不会打印共有属性



### （2）查看某个属性是否存在

```js
'toString' in obj

obj.hasOwnProperty('toString')

'toString' in obj && !obj.hasOwnProperty('toString')
```

* `in`查看是否在obj里，但是不能判断是自身属性还是共有属性
* `hasOwnProperty`函数可以判断是否是自身属性
* 两个结合一下就可以判断是自身数字能够还是共有属性



#### 注意点

`obj.xxx === undefined` 并不能判断某个属性是否存在，因为没有该属性是undefined，该属性的值存在且值为undefined也成立

![1650191501899](JS对象.assets\1650191501899.png)

### （3）查看单个属性

```js
obj['key']

obj.key
```

> 优先使用中括号语法，点语法会误导，以为key不是字符串
>
> 注意区分表达式是变量还是常量。
> 如果使用点语法，那么点后面一定是 string 常量。



#### 区分点

错误语法：

`obj[key]`  变量key值一般不为'key'

* 如果使用 [ ] 语法，那么 JS 会先求 [ ] 中表达式的值，

* obj.name 等价于 obj['name']，不等价与obj[name]    name是字符串而不是变量

  ```js
  let name = 'frank'
  
  obj[name] === obj['frank']
  ```

  

* **题目**：使person的所有属性被打印出来，log里面填什么

  选项：

  1.person.name

  2.person[name]

  ```js
  let list = ['name','age','gender']
  let person = {name:'frank',age:18,gender:'man'}
  for(let i = 0; i < list.length; i++){
      let name = list[i]
      console.log()
  }
  ```

  正确为2





## 3. 增、改（写）

有则改，无则增

### （1）单个修改

```js
let obj = {name:'frank'}  // name是字符串


//赋值方式
obj.name = 'jack'    //name是字符串

obj['name'] = 'jack'   

obj['na'+'me'] = 'jack'

let key = 'name';obj[key] = 'jack'  //key是变量，值为'name'
```

#### 错误点

```js
obj[name] = 'jack'    //错误，name是变量，值不确定

let key = 'name';obj.key = 'jack'  //错，obj.key === obj['key']
```





### （2）批量赋值

```js
Object.assign(obj,{age:18,gender:'man'})
```





# 三、JS对象分类

## 1. JS对象为什么分类

先看一段代码：12个正方形，有宽度、周长和面积

* 第一版：

  ```js
  let squareList = []
  let widthList = [5,6,5,6,5,6,5,6,5,6,5,6]
  
  for(let i = 0; i<12; i++){
      squareList[i] = {
          width:widthList[i],
          getArea(){
              return this.width * this.width
          },
          getLength(){
              return this.width * 4
          }
      }
  }
  ```

  > 浪费内存，循环是，每一次都会新生成一个函数，一共有24次函数，其中22个都是重复的

  暂时可以认为this就是当前对象



* 借助原型，把共用的两个函数放到原型里

  ```js
  let squareList = []
  let widthList = [5,6,5,6,5,6,5,6,5,6,5,6]
  
  let squarePrototype = {
      getArea(){
          return this.width * this.width
      },
      getLength(){
          return this.width * 4
      }
  }
  
  for(let i = 0; i<12; i++){
      squareList[i] = Object.create(squarePrototype)
      squareList[i].width = widthList[i]
  }
  ```

  > square代码太分散了

  

* 把代码抽离到一个函数里，然后调用函数

  ```js
  let squareList = []
  let widthList = [5,6,5,6,5,6,5,6,5,6,5,6]
   
  //构造函数
  function createSquare(width){
    let obj = Object.create(squarePrototype)  //以squarePrototype 为原型创建空对象
    obj.width = width
    return obj
  }
  
  let squarePrototype = {
      getArea(){
          return this.width * this.width
      },
      getLength(){
          return this.width * 4
      }
  }
  for(let i = 0; i<12; i++){
      squareList[i] = createSquare(widthList[i]) 
  }
  ```

  > squarePrototype原型和createSquare函数还是分散的



* 函数和原型结合

  ```js
  let squareList = []
  let widthList = [5,6,5,6,5,6,5,6,5,6,5,6]
   
  //构造函数
  function createSquare(width){
    let obj = Object.create(createSquare.squarePrototype)  //这里是先使用后定义吗？
    obj.width = width
    return obj
  }
  
  createSquare.squarePrototype = {  //把原型放到函数上
      getArea(){
          return this.width * this.width
      },
      getLength(){
          return this.width * 4
      },
      constructor:createSquare  //方便通过原型找到构造函数
  }
  for(let i = 0; i<12; i++){
      squareList[i] = createSquare(widthList[i]) 
  }
  ```

  > `console.log(squareList[i].constructor)`可以知道改对象是由谁构造的



* 把以上逻辑固定在new 操作符中

  ```js
  let squareList = []
  let widthList = [5,6,5,6,5,6,5,6,5,6,5,6]
   
  //构造函数
  function Square(width){
    this.width = width
  }
  //在原型上添加共有属性，也可以批量添加
  Square.prototype.getArea = function(){
      return this.width * this.width
  }
  Square.prototype.getLength(){
      return this.width * 4
  }
  
  for(let i = 0; i<12; i++){
      squareList[i] = new Square(widthList[i]) 
  }
  ```

  

* 最终版代码?

  构造函数

  ```js
  function Square(width){
      this.width = width
  }
  Square.prototype.getArea = function(){
      return this.width * this.width
  }
  Square.prototype.getLength = function(){
      return this.width * 4
  }
  
  let square = new Square(5)
  
  square.width
  square.getArea()
  square.getLength()
  ```

  

  **this**是构造出来的实例对象，在被构造之前不知道叫什么，this来代替

  

* class 重写

  ```js
  class Square{
     // static x = 1
      width = 0
      constructor(width){
          this.width = width
      }
      getArea(){
          return this.width*this.width
      }
      getLength(){
          return this.width*4
      }
  }
  
  let square = new Square(5)
  square.width
  square.getArea()
  square.getLength()
  ```





class语法

  语法1：

  ```js
  class Person{
      sayHi(name){}
      // 注意，一般我们不在这个语法里使用箭头函数
  }
  
  
  //等价于
  function Person(){}
  Person.prototype.sayHi = function(name){}
  ```

  语法2：

  ```js
  class Person{
    sayHi = (name)=>{} // 注意，一般我们不在这个语法里使用普通函数，多用箭头函数
  }
  
  
  // 等价于
  function Person(){
      this.sayHi = (name)=>{}
  }
  ```

语法1和语法2的区别在于一个使用箭头函数，一个使用普通函数

箭头函数没有this，所以不会挂到构造函数的prototype属性上，而是会直接挂到新生成的对象上

![1650193494270](JS对象.assets\1650193494270.png)







## 2. JS对象分类

对象需要分类：

* 有很多对象拥有一样的属性和行为，需要把它们分为同一类，创建类似对象就很方便

* 但是还有很多对象拥有其他的属性和行为，所以就需要不同的分类
  * 比如 Array 、Function 、Date、RegExp等

* Object创建出来的对象是最没特点的对象



类是针对于对象的分类，有无数种









## 3. new

new X() 做了四件事：

* 创建空对象

* 为空对象关联原型，原型地址指定为 `X.prototype`

* 将空对象作为**`this`**关键字运行构造函数

* `return this







# 四、数组对象

## 1. JS数组定义

JS其实没有真正的数组，只是用对象模拟数组

JS的数组不是典型数组



* 典型的数组：
  * 元素的数据类型相同
  * 使用连续的内存存储
  * 通过数字下标获取元素



* JS数组

  * 元素的数据类型可以不同

  * 内存不一定是连续的（对象是随机存储的）

  * 不能通过数字下标，而是通过字符串下标

  * 这意味着可以有任何key

    > 通过数字取到数组元素是因为JS会调用1.toString方法先变成字符串再传进去
    >
    > ```js
    > let arr = [1,2,3]
    > arr[(1).toString()] ===  arr[1]
    > arr['xxx'] = 1
    > ```



![1650195144580](JS对象.assets\1650195144580.png)



> 有一个问题：为什么`for`遍历和直接打出`arr`，数组的`length`都为`3`，但是用`Object.keys()`打印出`keys`就有四个呢？
>
> ​        答：最后添加的`'xxx'`是为arr添加的一个属性，而不是表示存储顺序的下标，所以，下标的0、1、2、3等是不能更改的，所以`keys`有四个，但是`length`没有增加







## 2. 创建数组

### （1）声明一个数组

```js
let arr = [1,2,3]

let arr = new Array(1,2,3) // 元素为1,2,3，是上一句的正规写法

let arr = new Array(3)   //长度为3
```





### （2）衍生一个数组

```js
//合并
arr1.concat(arr2)

//截取
arr1.slice(1)  //从第二个元素开始截取
arr1.slice(0)  // 全部截取，复制一个数组
```

* 以上两种方法都不会改变原数组，只会生成新数组
* JS只提供**浅拷贝**



### （3）转化一个数组

* **`split()`**分隔，使用**字符串**创建一个数组，

  ```js
  let arr = '1,2,3'.split(',')
  
  let arr = '1,2,3'.split('') //空字符串
  ```

  ![1650196472188](JS对象.assets\1650196472188.png)





* **`Array.from()`**转化

  把不是数组的东西尝试变成数组

  从哪里得到一个数组

  并不是都可以变成数组，要有一定的条件：

  * 一个对象有`0,1,2,3`，这样的下标和`length`就可以变成数组

```js
//数字
Arrey.from(123)        //F

//字符串
Arrey.from('123')     //T
Arrey.from('name')    //T

//布尔
Array.from(true)      //F

//对象
Array.fron({0:'a',1:'b',2:'c'})             //F
Array.fron({0:'a',1:'b',2:'c',length:3})    //T
```

 



### 伪数组

* 没有数组共有属性的数组就是伪数组

  （伪数组的原型链中并没有数组的原型）

  ```html
  <body>
      <div>1</div>
      <div>2</div>
      <div>3</div>
  </body>
  ```

  ```js
  let divList = document.querySelectorAll('div')
  console.log(divList)
  ```

  会发现是数组的形式，但是点看并没有数组的方法，使用push、pop等数组方法会报错

  ![1650203391857](JS对象.assets\1650203391857.png)

  只是创建了一个对象，没有构建数组的原型

  

* 使用`Array.from`自己构建

  `let divArray = Array.from(divList)`

  打印出`divArray`就会有数组的原型链

  ![1650203303187](JS对象.assets\1650203303187.png)





## 3. 增删改查数组中的元素

### （1）删

#### 1）推荐用法

```js
//删除头部元素
arr.shift ()     //提档

//删除尾部元素
arr.pop()  //弹出

//删除中间元素
arr.splice(index,1)            //删除index处的一个元素
arr.splice(index,2,'x')        //删除index处的2个元素并在删除位置添加‘x’
arr.splice(index,1,'x','y')    //在删除位置添加‘x’和‘y’
```

* 以上三种方法**返回**的都是被删元素
* `arr.shift()`和`arr.pop()`不接受参数，只删除一个元素



####  2）不推荐的方法

可以删除，会有习惯使用到以下方法，但不推荐使用

* 使用对象的方法：`delete`

  ```js
  let arr = ['a','b','c']
  delete arr['0']
  arr          //[empty,'b','c'],length:3
  ```

  * 数组的长度并没有变

  * 稀疏数组 ：长度比元素多，没有好处，只有bug

  

* 通过改`length`删元素

  `arr.length = 1`

  是可以的

  会从数组尾部删除超出length的元素





### （2）查

#### 1）遍历

##### ① for

```js
for (let i = 0; i < arr.length; i++){
    console.log(`${i}:${arr[i]}`)
}
```



##### ②forEach

```js
arr.forEach(function(item,index){
    console.log(`${index}:${item}`)
})
```

* 理解forEach

  手写forEach

  ```js
  function forEach(array,fn){
      for(let i=0;i<array.length;i++){
          fn(array[i],i,array)
      }
  }
  
  forEach(['a','b','c'],function(x,y,z){console.log(x,y,array)})
  ```

  forEach用for访问array的每一项

  对每一项调用fn(array[i],i,array)



##### ③for和forEach区别

* for是个关键字，功能更强大

  跟着块级作用域

  for里面有break和continue



* foreach只是个函数

  跟着函数作用域



#### 2）单独查找/查看

```js
//查看单个属性
let arr = [111,222,333]
arr[0]


//查找某个元素是否在数组里
arr.indexOf(item)   //存在返回索引，否则返回-1


//条件查找
arr.find(item=>item%2 === 0)   //找第一个偶数
arr.findIndex(item => item%2 === 0)// 找第一个偶数的索引
```

`find()`和`findIndex()`都只会返回找到的第一个



#### 3）不推荐的方法

可以查找/查看，会有习惯使用到以下方法，但不推荐使用

* 对象方法查看所有属性名和值

  ```js
  let arr = [1,2,3,4,5];arr.x = 'xxx'
  
  Object.keys(arr)      //['0','1','2','3','4','5','x']
  Object.values(arr)     //[1,2,3,4,5,'xxx']
  ```

  

* `for(let…in…){}`

  ```js
  for(let key in arr){
      console.log(`${key}:${arr[key]}`)
  }
  ```

  

不适用，以上两种方法都会把属性以及索引都找出来



* 索引越界

  使用并不存在的索引

  举例：

  * `arr[arr.length]  === undefined`

  * `arr[-1] === undefined`

    ```js
    for(let i= 0;i<=arr.length;i++){
        console.log(arr[i].toString())
    }
    ```

    报错`Cannot read property 'toString'of undefined`

    意思是读取了`undefined`的`toString`属性，`x.toString()`其中`x`如果是`undefined`就报这个错

  `arr[index]`最大只能到`arr.length-1`,`index`是从`0`开始的









### （3）增

#### 1）推荐用法

```js
//尾部
arr.push(newItem) 
arr.push(item1,item2)   


//头部
arr.unshift(newItem)
arr.unshift(item1,item2)


//中间
arr.splice(index,0,'x')
arr.splice(index,0,'x','y')
```

`arr.push()`和`arr.unshift()`都会修改arr，返回新长度



#### 2）不推荐用法

`arr[x]= 'x' `( 单个元素单独赋值）

不推荐这样加，因为如果改一个较大的数字，就直接length也会变很大,就搞成系数数组了

![1650208303412](JS对象.assets\1650208303412.png)



### （4）改

#### 1）reverse

反转顺序

`arr.reverse()`

![1650212873011](JS对象.assets\1650212873011.png)

修改了原来的数组，并没有生成新的数组，返回修改后的新数组

* 题目：把字符串反转

  ```js
  let s = 'abcdef'
  s.split('').reverse().join('')
  ```

  

#### 2）sort

自定义顺序

`arr.sort()`

接受一个函数，指定比较方法

可以对**数组**内任意可以用**数值**表示的元素进行比较

sort默认会通过**ASCII**码进行先后排序

**修改**了原来的数组，不会生成新的数组，返回修改后的数组

* 可以使用**b-a**的方式进行从大到小的排序

* 可以通过函数，给元素赋值，指定比较方法

```js
let arr = [4,2,7,9,1]
arr.sort()  //[1,2,4,7,9]

arr.sort(function(a,b){
    if(a>b){
        return 1
    }else if(a === b){
        return 0
    }else{
        return -1
    }
}) // 给定参考值，从小到大排序，等价于
arr.sort((a,b) => a-b)

//从大到小
arr.sort((a,b) => b-a)
```





* 对象中的数值进行比较

```js 
let arr = [{name:'小红',score:95},{name:'小明',score:98},{name:'小兰',score:100}]

arr.sort(function(a,b){
    if(a.score>b.score){
        return 1
    }else if(a.score === b.score){
        return 0
    }else{
        return -1
    }
})//等价于
arr.sort((a,b)=>a.scaor-b.score)
```





* 字符串按长度排序

```js
let s = ['abcde','abcdeffg']
s.sort((a,b)=>a.length-b.length)
```





* 字符串按先后顺序排序（ASCII码）

```js
let s = 'gsjgaklk'
Array.from(s).sort().join('').toString()
```





#### 3）map

提供一一对应的映射关系

n变n

返回新数组，**不改变**原数组

```js
let arr = [1,2,3,4,5,6,7]
arr.map(item => item*item)

//一一对应，返回平方
```





#### 4）filter

n变少

返回新数组，**不改变**原数组

判断真假，真返回，假丢弃

```js
arr.filter(item => item %2 === 0)   

//返回偶数
```





#### 5）reduce

n变1

返回新数组，**不改变**原数组

`reduce` 接受两个参数，第一个为函数，表示想要如何叠加，第二个为reduce的初始值

```js
arr.reduce((sum,item)=>{return sum+item},0)

//返回数组中各值的和
```



* `reduce`替换`map`

  ```js
  arr.reduce((result,item) => {return result.concat(item*item)},[])
  ```

  > `push `代替`concat`是什么结果?
  >
  > 会报错，因为`push`返回的是数组的长度，而不是数组，不能循环push，第二次push就是`长度.push()``
  >
  > ``concat`返回的是新数组

  

  

* `reduce`替换`filter`

  ```js
  arr.reduce((result,item)=>result.concat(item %2 === 0 ? [] : item),[])
  //等价于
  arr.reduce((result,item)=>{
      if (item %2 === 0){
          return result
      }else {
          return result.concat(item)
      }
  },[])
  ```

  

#### 6）基础写法

对已有的元素直接进行修改

`arr[1] = 666 `

 





## 4. 题目：

### （1）数字变日期

```js
let arr = [0,1,2,2,3,3,3,4,4,4,4,6]
let arr2 = arr.map(补全代码)
console.log(arr2) // ['周日', '周一', '周二', '周二', '周三', '周三', '周三', '周四', '周四', '周四', '周四','周六']
```



答：

* switch case

  ```js
  let arr = [0,1,2,2,3,3,3,4,4,4,4,6]
  let arr2 = arr.map(a=>{
      switch(a){
          case 0:
              return "周日";
              break;
          case 1:
              return "周一";
              break;
          case 2:
              return "周二";
              break;
          case 3:
              return "周三";
              break;
          case 4:
              return "周四";
              break;
          case 5:
              return "周五";
              break;
          case 6:
              return "周六";
              break;    
     }
  })
  console.log(arr2)
  ```

* 对象

  ```js
  let arr = [0,1,2,2,3,3,3,4,4,4,4,6]
  
  
  let arr2 = arr.map( (a)  => {return{0:'周日',1:'周一',2:'周二',3:'周三',4:'周四',5:'周五',6:'周六'}[a]})
  
  //等价于
  
  let arr2 = arr.map(a =>{
      const day = {0:'周日',1:'周一',2:'周二',3:'周三',4:'周四',5:'周五',6:'周六'}
      return day[a]
  })
  
  
  console.log(arr2)
  ```

  ```js
  let arr = [0,1,2,2,3,3,3,4,4,4,4,6]
  let arr2 = arr.map(a =>{
      const day = {0:'周日',1:'周一',2:'周二',3:'周三',4:'周四',5:'周五',6:'周六'}
      return day[a]
  })
  console.log(arr2)
  ```

  ```js
  let arr = [0,1,2,2,3,3,3,4,4,4,4,6]
  
  let obj ={
    0:"周日",
    1:"周一",
    2:"周二",
    3:"周三",
    4:"周四",
    5:"周五",
    6:"周六",
  }
  
  let arr2 = arr.map((item)=> obj[item])
  console.log(arr2) 
  ```

  

> 对于这道题的一些想法：
>
> ​        看到题目的第一时间想到的是数字Number转换成日期Date，
>
> ​        Date中有个getDay()方法可以返回星期，
>
> ​       用到这个方法的前提是把数字换成日期，但是单个的数字怎么变成日期？
>
> ​       `getDay()`返回星期的方法是要有具体的日期，比如1995.11.25这天是星期几
>
> ​       所以这个方法不适用
>
> ​       所以，这个问题就不是数字换成日期，而是数组中元素一一对应字符串进行输出
>
> ​       而要做的就是数组中的元素与字符串如何对应







### （2）找出所有大于60分的成绩

```js
let scores = [95,91,59,55,42,82,72,85,67,66,55,91]
let scores2 = scores.filter(补全代码)
console.log(scores2) //  [95,91,82,72,85,67,66, 91]
```



答：

```js
let scores = [95,91,59,55,42,82,72,85,67,66,55,91]
let scores2 = scores.filter(i => i>60)
console.log(scores2) 
```





### （3）算出所有奇数之和

```js
let scores = [95,91,59,55,42,82,72,85,67,66,55,91]
let sum = scores.reduce((sum, n)=>{
  补全代码
},0)
console.log(sum) // 奇数之和：598 
```



答：


```js
let scores = [95,91,59,55,42,82,72,85,67,66,55,91]
let sum = scores.reduce((sum, n)=>{
    if(n%2 !== 0){
        sum +=n
    }
    return sum
},0)
console.log(sum) 
```

```js
let scores = [95,91,59,55,42,82,72,85,67,66,55,91]
let sum = scores.reduce((sum, n)=>{return n%2 !== 0 ? sum + n : sum},0)
console.log(sum) 
```







### （4）数据变换

```js
let arr = [
    {名称:'动物', id:1, parent:null}
    {名称:'狗', id:2, parent:1}
    {名称:'猫', id:3, parent:1}
]
```

数组变成对象

```js
{
    id:1, 名称:'动物',children[
        {id:2, 名称:'狗', children:null},
        {id:3, 名称:'猫',children:null}
    ]
}
```

解读题目：

数组中有三个**独立**对象，三个对象互相关联，一个爸爸，两个儿子，各有名称以及id指代**自己**，**parent**属性指向爸爸

转换后得到的是**一个对象**，父对象中有个**数组**，数组中分别是两个儿子对象，同时没有了**parent**属性，而是有了**children**属性，儿子数组就**在**children属性中

三个变一个，用reduce



答：

```js
let arr = [
    {名称:'动物', id:1, parent:null}
    {名称:'狗', id:2, parent:1}
    {名称:'猫', id:3, parent:1}
]

let animal = arr.reduce((result,i)=>{
    if(i.parent === null){
        result.id = i.id
        result['名称'] = i['名称']
    }else{
        result.children.push(i)
        delete i.parent
        i.children = null
        
    }
    return result
},{id:null,名称:null,children:[]})

console.log(animal)
```



![1650263063016](JS对象.assets\1650263063016.png)

为什么数组中的名称在前，id在后呢，这个数组是不是也可以用一个嵌套的reduce实现占位，让id在前，名称在后呢？参考下图，但是我暂时不会做，下图也没看懂



![1650262970698](JS对象.assets\1650262970698.png)



2022.4.19  今天看到针对Array又出了新的非破坏性API，toSorted()  / toReversed()  /  toSpliced() / with()



# 五、函数对象

函数有具名函数，匿名函数，箭头函数，构造函数等

## 1. 定义函数



### （1）具名函数

```JS
function 函数名(形式参数1，形式参数2){
    语句
    return 返回值
}
```



### （2）匿名函数

也叫函数表达式（仅指代等号右边的部分）



```js
let a = function(x,y){return x+y}
```

> 没有函数名
>
> 但是需要一个**变量**容纳它
>
> 作用域也仅在等号右边
>
> 其它地方想要调用函数只能通过变量



### （3）具名+匿名

```js
let a = function fn(x,y){return x+y}
```

> 变量a 存储着函数fn的**地址**
>
> 直接调用fn会报错
>
> 因为函数的声明在等号右边时，函数的**作用域**只在等号右边



### （4）箭头函数

```js
let f1 = x => x*x
let f2 = (x,y) => x+y                //圆括号不能省
let f3 = (x,y) => {return x+y}       //花括号不能省
let f4 = (x,y) => ({name:x,age:y})   // 直接返回对象会出错，要加圆括号,不加圆括号会被认为是label代码块，在字符串加引号呢？
```

>多个参数不能省圆括号
>
>return不能省花括号
>
>多个语句要在花括号外加圆括号
>
>箭头函数没有this









### （5） 构造函数

#### 1）栗子：

```js
function Person(name,gender){
    this.name = name
    this.gender = gender
}
Person.prototype.age = function(){console.log('18')}
Person.prototype.nationality = function(){console.log('中国')}
Person.prototype.skin = 'yellow'


let person1 = new Person('小红','女')
let person1 = new Person('小明','男')
```



![1650337075535](JS对象.assets\1650337075535.png)

> Person是构造函数
>
> 指定其实例可以接受两个参数，分别是name和gender
>
> 同时还有三个属性可以供其构造出来的实例使用，age、nationality两个函数，和skin一个属性
>
> 实例构造出来时就可以通过上溯原型使用其构造函数定义的函数和属性
>
> 可以看到被构造出来的person1和person2都有name和gender两个自身属性，原型中有age、nationality和skin三个共有属性



#### 2）定义

能构造出实例对象的函数就是构造函数



共用可以是函数，可以是属性



构造函数X

* X函数本身负责给对象本身添加属性

* X.prototype 对象负责保存手动写给对象的共用属性

* 与构造函数提供的原型的prototype是不同的

  ![1650338422712](JS对象.assets\1650338422712.png)









* 每个函数都有prototype属性 （除了箭头函数）
  
* 区别于其构造函数赋予的原型对象存在的prototype属性
  
* 每个prototype都有constructor属性

  * constructor属性保存了对应的函数的地址

  * 意思就是指向它的构造函数，看上图↑

  * 如果是构造函数，那么它的constructor就指向它自己

    ![1650340235685](JS对象.assets\1650340235685.png)



```js
function f1(){}

console.dir(f1)

f1.prototype.constructor === f1
```







* 是对象但不是函数，那么它一般来说没有prototype属性，但这个对象一般一定会有`__proto__`属性，也就是原型

* 是函数但不是构造函数，它的prototype属性暂时没用

  * 用来存放构造函数写给它的共有属性

  * 构造函数的prototype属性用来存放给实例使用的共有属性

* 这个prototype是属性，原型的[[Prototype]]只是利用来存放原型的地址

 

#### 3）特殊对象的构造函数

可以通过`constructor`属性看出构造者

* **对象**是由函数构造的

  

  ```js
  Object.__proto__                    
      === Function.prototype    //对象的原型是其构造函数Function()提供的
  Object.__proto__.__proto__           
      === Function.prototype.__proto__ 
      === Object.prototype     //对象的原型对象是由Object()提供的
  Object.__proto__.__proto__.__proto__ 
      === Function.prototype.__proto__.__proto__ 
      === Object.prototype.__proto__  
      === null                 //原型对象的原型是所有对象的根，null
  ```

  区分构造函数的原型对象以及原型对象的原型



* **函数**是由函数构造的，函数的原型也是对象

  浏览器创造了函数，并指定为自己构造了自己



* **window**是Window构造的



![1650351906728](JS对象.assets\1650351906728.png)





问题：

new function不可用，咱也不知道啥原因

就去文档里抄了代码

![1650339140743](JS对象.assets\1650339140743.png)

就得到了报错

![1650339170738](JS对象.assets\1650339170738.png)

报错里给的链接也点不开，还不被认为是JS 

Emmm。。。

![1651891388337](JS对象.assets\1651891388337.png)

这是什么！！！！

神奇





## 2. 函数的要素

* 调用时机

* 作用域

* 闭包

* 形式参数

* 返回值

* 调用栈

* 函数提升

* arguments（除了箭头函数）

* this（除了箭头函数）



### （1）函数fn、函数调用fn()



```js
let fn = () => console.log('hi')
fn
// 没有结果，因为fn没有执行

let fn = () => console.log('hi')
fn()
//打印出hi
```





```js
let fn = () => console.log('hi')
let fn2 = fn
fn2()
```

结果：

* fn保存了匿名函数的地址
* 这个弟子被复制给了fn2
* fn2()调用了匿名函数
* fn和fn2 都是匿名函数的引用而已
* 真正的函数既不是fn也不是fn2

  



### （2）调用时机

时机不同结果不同

```js
let a = 1
function fn(){
    console.log(a)
}//不知道打印出多少，没有调用代码
```

```js
let a = 1
function fn(){
    console.log(a)
}
fn()//打印出1
```

```js
let a = 1
function fn(){
    console.log(a)
}

a = 2
fn()//打印出2
```

```js
let a = 1
function fn(){
    console.log(a)
}

fn()
a = 2  //打印出1
```

```js
let a = 1
function fn(){
    setTimeout(()=>{console.log(a)}) 
}

fn()
a = 2//打印出2
```

```js
let i = 0
for(i=0; i<6; i++){
    setTimeout(()=>{
        console.log(i)
    },0) 
}//打印出6个6
```

```js
for(let i=0; i<6; i++){
    setTimeout(()=>{
        console.log(i)
    },0) 
}//打印出0、1、2、3、4、5
```

JS在for和let一起用的时候，每循环一次都会重新声明，多创建一个i

这样setTimeout就失去作用了



* 问题：for、let、setTimeout打印出0,1,2,3,4,5

  答1：即时函数

  ```js
  let i=0
  for( i=0 ; i<6; i++) {
     !function(i){
         setTimeout(()=>{
             console.log(i)
         }, 0)
     }(i)
  }
  ```

  答2：中间变量

  ```js
  let a = 0
  for (a = 0;a<6;a++){
      let j = a
      setTimeout(()=>{
          console.log(j)
      },0)
  }
  ```

  



### （3）作用域 

每个函数都会默认创建一个作用域

```js
function fn(){
    let a = 1
}
console.log(a) //a不存在
```

```js
function fn(){
    let a = 1
}
fn()
console.log(a)//a还是不存在
```

因为let的作用域只在花括号里



#### 全局变量和局部变量

在顶级作用域声明的变量是全局变量，window是属性是全局变量，其他斗志局部变量



#### 函数可嵌套，作用域也可嵌套

```js
function f1(){
    let a = 1
    function f2(){
        let a = 2
        console.log(a)
    }
    console.log(a)
    a = 3
    f2()
}
f1()
```

#### 作用域规则

* 如果多个作用域有同名变量a，那么查找a的声明时，就向上取最近的作用域，简称**就近原则**

* 查找a的过程与函数执行无关     **静态作用域**  与之相反的有词法作用域也叫动态作用域   

* 但a的值与函数执行有关

```js
function f1(){
    let a = 1
    function f2(){
        let a = 2
        function f3(){
            console.log(a)
        }
        a = 22
        f3()
    }
    console.log(a)
    a = 100
    f2()
}
f1()
```





### （4）闭包

如果一个函数用到了外部的变量，那么这个函数加这个变量就叫做闭包

左边的`a`和`f3`组成了闭包



### （5）形式参数

形式参数的意思就是非实际参数

```js
function add (x,y){
    return x+y
}
add(1,2)
```

其中`x`和`y`就是形参，调用`add()`时，`1`和`2`是实际参数，会被赋值给x y



* 值传递和地址传递：

  没有值和地址之分，不管是对象的地址，还是数值，统一按照内存图直接复制



* 形参可认为是变量声明

上面的代码近似定价于下面的代码

```js
function add(){
    var x = arguements[0]   //第0个参数
    var y = arguements[1]
    return x+y
}
```

形参可多可少，只是给参数取名字



### （6）返回值

每个函数都有返回值

```js
function hi(){console.log('hi')}
hi()//没写return，所以返回值是undefined
```



```js
function hi(){return console.log('hi')}
hi()//返回值是console.log('hi')的值，即undefined
```



* 函数执行完后才会返回

* 只有函数有返回值

* `1+2`的值为`3`，`1+2`没有返回值 







### （7）调用栈

JS引擎在调用一个函数前需要把函数所在的环境push到一个数组里，这个数组叫调用栈

等函数执行完了，就会把环境弹（pop）出来

然后return到之前的环境，继续执行后续代码



举例

```js
console.log(1)
console.log('1+2的结果为' + add(1,2))
console.log(2)
```

进入log函数，压栈，打印1，弹栈

进入第二行，压栈，执行add(1,2)，压栈，记录等会回到第2行21列，得到3，弹栈，进入log函数，压栈，运行完之后弹栈





#### 递归函数

阶乘

```js
function f(n){
    return n !== 1 ? n*f(n-1) 1
}
```

```
f(4)
=4*f(3)
=4*(3*f(2))
=4*(3*(2*f(1)))
=4*(3*(2*(1)))
=4*(3*(2))
=4*(6)
=24
```

递归函数的调用栈很长

调用栈最长有多少

```js
function computerMaxCallStackSize(){
    try{
        return 1 + computerMaxCallStackSize();
    }catch(e){
        //报错说明stack overflow 了
        return 1
    }
}
```



Chrome   12578

FireFox 26673

Node 12536

* 爆栈

  如果调用栈中压入的帧过多，程序就会崩溃







### （8）函数提升

```js
function fn(){}
```

不管把具名函数声明放到哪里，都会跑到第一行



```js
let fn = function(){}
```

这是赋值，右边的匿名函数不会提升



```js 
let fn = function f1(){}
```

会提升吗？不会！请看函数声明那一节



### （9）arguments和this

除了箭头函数，其它函数都有

```js
function fn(){
    console.log(arguements)
    console.log(this)
}
```

#### 传arguements

* 调用fn即可传arguements
  * `fn(1,2,3)`那么`arguements`就是`[1,2,3]`伪数组

* 如果不给任何值，`this`默认指向`window`

#### 传this

* 目前可以用`fn.call(xxx,1,2,3)`传`this`和`arguements`
* 而且`xxx`会被自动转化成对象   ` xxx`是`this`，`1,2,3`是`arguements`



* this是什么呢？

  **this**是构造出来的实例对象，在被构造之前不知道叫什么，this来代替



this是隐藏参数

arguements是普通参数



```js
let person = {
    name:'frank',
    sayHi(){
        console.log(`你好，我叫`+ person.name)
    }
}
```

> 在声明一个函数的时候可以用一个**变量**得到这个对象的引用 ，所以`person`不是先使用后声明，没有违反规则
>
> 可以用保存了对象地址的变量获取`'name'`
>
> 这种办法简称为**引用**
>
> 函数调用时，把`this`给出来，把`person`给`this`，就相当于把`person`给出来
>
> `this`就是最终调`sayHi`的对象





# 原型链

## 1. 原型概念

**原型是个对象——原型对象**

原型解释了为何一个对象会拥有定义在其他对象中的属性和方法



JS中每个对象都有一个**隐藏属性**

这个隐藏属性存着其**共有属性**组成的**对象**的**地址**

这个共有属性组成的对象叫做**原型**



> 每个对象都有原型
>
> 原型里存着对象的共有属性
>
> 原型的地址存在隐藏属性里
>
> 这个隐藏属性**暂时**就叫`__proto__`（只是帮助理解，没有实际意义）



对象的原型是定义在其构造函数之上的`prototype`属性上，被构造出来的对象通过`__proto__`属性复制了原型的地址



`__proto__`属性在对象实例和它的构造器之间建立一个链接（`__proto__`属性是从构造函数的`prototype`属性派生的）



既然原型也是个对象，那么原型也可能有原型



## 2.举例解释：

### （1）普通对象

```js
let obj = {}

let obj = new Object()
```

`obj实例`是由`Object函数`构造的

所以，`obj`可以在`Object`的`prototype`属性记录的**地址**中寻找**继承**方法和属性（`toString / constructor / valueOf `等`Object`构造出的对象都可以使用的属性），其中有一个`__proto__`属性复制了`Object.prototype`存储的地址

`obj.__proto__ === Object.prototype`

obj 的原型也是一个对象，存储着Object构造出来的实例都可以使用的属性





**那么`Object.prototype`有原型吗？**

答案是有，但是值为null

也就是Object.prototype就是所有对象的根了

`Object.prototype.__proto__=== null`



`obj.__proto__`是obj的原型，也就所有对象的原型，是对象的根，在往上就是null



![1649912691447](JS对象.assets\1649912691447.png)

![1649912814291](JS对象.assets\1649912814291.png)



### （2）数组对象

```js
let arr = []

let arr = new Array()
```



arr实例是由Array函数构造的，

所以`arr`的`__proto__`属性是从Array的prototype属性中继承的，并且两个存储了一样的地址，指向了arr的原型对象（所有Array函数构造出来的实例可以共同使用的属性的集合）

所以`arr.__proto__ === Array.prototype`

arr的原型对象也有原型

原型对象的构造函数就是Object

所以`Array.prototype.__proto__ === Object.prototype`

`Object.prototype.__proto__ === null`

![1649914171413](JS对象.assets\1649914171413.png)



![1649914279534](JS对象.assets\1649914279534.png)





### （3）函数对象

 ```js
function fn(){}
 ```



fn实例是由Function函数构造的，

所以fn 的原型对象就是`fn.__proto__ === Function.prototype`指向的共有属性

这个原型对象也有原型

`Function.prototype.__proto__  === Object.prototype`







![1649918754009](JS对象.assets\1649918754009.png)

![1649918865997](JS对象.assets\1649918865997.png)







## 3. 原型链

每个对象拥有一个**原型对象**，对象以其原型为模板、从原型继承方法和属性。原型对象也可能拥有原型，并从中继承方法和属性，一层一层、以此类推。这种关系常被称为**原型链 (prototype chain)**



举例：

`arr` 如果要使用`hasOwnProperty()`方法，自身没有定义这个方法，就去`Array.prototype`指向的对象里找，没有就在上一层到`Object.prototype`指向的对象里找





## 4. 总结

构造函数指定了其构造实例可以使用的一系列属性和方法，这些属性和方法叫做**原型对象**，

原型对象的地址存储在`prototype`属性中，`prototype`属性派生出`__proto__`属性，实例可通过`__proto__`属性链接到构造函数给出的原型对象上

原型对象也有原型，一层一层上溯，一层一层链接，就是**原型链**

**读**，会上溯到原型链，**写**，只会在原型链最上层——实例那一层另外添加，不会更改到原来的原型链

原来的原型链也可以更改，但是了解原型是什么之后还要改吗？🙂🙂🙂







你是谁构造的，你的原型就是谁的prototype属性对应的对象

`对象.__proto__ === 其构造函数.prototype`





`obj` => 自写属性 => `Object.prototype` => null

`arr` => 自写属性 =>` Array.prototype` => `Object.prototype` => null

`fn` => 自写属性 =>` Function.prototyoe` => `Object.prototype` => null







## 5. 细节

* `prototype `和`__proto__`区别
  * 存着相同的原型地址
  * prototype挂在函数上，Object
  * proto挂在每个新生成的对象上

* 改（写）不走共有属性，只能读到共有属性
* `__proto__`只是存储共有属性的地址，没有实际意义，不同浏览器可能叫法不同  
* `console.dir `输出结构

* [`Object.prototype.__proto__`已废弃](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)

可以借用   帮助理解



[原型文档](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Object_prototypes)







Object.prototype是本来就有的，是所有对象的根，值为null，没有构造函数

它没有原型







## 6. 原型应用

### （1）删

`xxx.__proto__ = null`

但是不要删，删了就不能用了，毕竟JS是“基于原型的语言”





### （2）查

* 查看所有原型对象：

`console.dir(obj)`  以目录形式打印

` obj.__proto__`



* 判断是否有某个属性：

  `'xxx' in obj` 

  可以判断`xxx`属性是否在`obj`里面，不一定是自身属性还是原型属性

* 判断一个属性是自身的还是共有的

  `obj.hasOwnProperty('toString')`

  不是自身属性就报错，原型属性也报错



### （3）改或增

读会走原型，写不会走原型

无法通过自身修改或增加共有属性

修改后会在原型上再加一层原型，不会修改到原有的原型



```js
let obj = {},obj2 = {}   //同为对象，共有toString属性
obj.toString = 'xxx'     //只会改在obj自身属性，obj2.toString还是在原型上
```

![1650628564011](JS对象.assets\1650628564011.png)



obj原型中的toString 方法依旧还有，只是在最上层又增加了一个toString属性，当调用obj的toString时，在自身属性中找到就不会走到原型的toString

而obj2的toString依旧是原型中的toString，没有改变



可修改方法：

```js
//两个等价
obj.__proto__.toString = 'xxx'    // 不推荐使用__proto__
Object.prototype.toString = 'xxx'
```

这样就可以修改原型中的属性了

**但是！！**

一般不要修改原型，会引发很多问题，很影响性能



应用：

不推荐使用`__proto__`

```js
let obj = {name:'frank'}
let obj2 = {name:'jack'}
let common = {kind:'human'}
obj.__proto__ = common
obj2.__proto__ = common
```



推荐使用Object.create

```js
let obj = Object.create(common)
obj.name = 'frank'
let obj2 = Object.create(common)
obj2.name = 'jack'
```

要改一开始就改，别后来再改，







教程

入门https://wangdoc.com/javascript/

进阶https://book.douban.com/subject/26351021/

http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html

浏览器工作原理https://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/#1_1   各种资料合集，原文文档，闲的没事干可以看

v8引擎https://blog.sessionstack.com/how-javascript-works-inside-the-v8-engine-5-tips-on-how-to-write-optimized-code-ac089e62b12e

  知乎方应杭JS 中 __proto__ 和 prototype 存在的意义是什么？ https://www.zhihu.com/question/56770432/answer/315342130

知乎方应杭JS中的new到底是干什么的？https://zhuanlan.zhihu.com/p/23987456

知乎方应杭你可以不会 class，但是一定要学会 prototypehttps://zhuanlan.zhihu.com/p/35279244

