---
title: JavaScript 进阶
keywords: JavaScript
desc: JavaScript学习笔记
date: 2024-06-16
class: heading_no_counter
tags: 笔记, JavaScript
---

## day1-作用域&解构&箭头函数

### 作用域

**- 局部作用域**

- 函数作用域：函数执行完毕，内部的变量就被清空
- 块作用域：let声明的变量和const声明的常量产生块作用域、var不会产生块作用域

**- 全局作用域**

- 即`<script>`标签和.js文件的【最外层】
- 全局作用域中声明的变量在其他任何作用域都可以被访问

#### **- 作用域链**

- 本质上是底层的**变量查找机制**
- 在函数被执行时，会**优先查找当前**函数作用域中的变量
- 若当前作用域查找不到则会依次**逐级查找父级作用域**直到全局作用域
- -》嵌套关系的作用域串联起来形成了作用域链；子作用域能访问父作用域，但是不能反过来

#### **- JS垃圾回收机制(GC)**

JS中内存的分配和回收都是自动完成的，内存在不使用的时候会被垃圾回收器自动回收

内存的生命周期：

- 内存分配：声明变量、函数、对象时，系统自动为其分配内存
- 内存使用：读写内存，即使用变量、函数等
- 内存回收：使用完毕，由垃圾回收器自动回收不再使用的内存

​		内存泄漏：程序中分配的内存由于某种原因未释放或无法释放

堆栈空间分配区别：

- 栈（操作系统)∶由操作系统自动分配释放函数的参数值、局部变量等，基本数据类型放到栈里面。
- 堆〈操作系统)∶一般由程序员分配释放，若程序员不释放，由垃圾回收机制回收。**复杂数据类型放到堆里面**。

两种常见的**浏览器垃圾回收算法**:

- 引用计数法

  IE采用的引用计数算法，定义“内存不再使用”，就是看一个对象是否有指向它的引用，没有引用了就回收对象算法:
  1．跟踪记录被引用的次数
  2．如果被引用了一次，那么就记录次数1,多次引用会累加++

  3.如果减少一个引用就减1 --
  4．如果引用次数是0，则释放内存

  ```javascript
  let person = {		// 引用次数0->1
  	age: 18,
  	name: '佩奇'
  }
  let p = person		// 引用次数1->2
  person = 1			// 引用次数2->1
  p = null			// 引用次数1->0，此时该对象被回收
  ```

  存在一个致命问题：嵌套引用，即若两个对象相互引用，即使它们不再使用也不会被回收，导致内存泄漏

- 标记清除法

​	现代浏览器通用的大多是基于标记清除算法的某些改进算法，总体思想都是一致的。核心:

 	1. 标记清除算法将“**不再使用的对象**”定义为“**无法达到的对象**”。
 	2. 就是从根部（在JS中就是全局对象）出发定时扫描内存中的对象。凡是能从根部到达的对象，都是还需要使用的。
 	3. 那些无法由根部出发触及到的对象被**标记为不再使用，稍后进行回收**。

#### **- 闭包**

一个函数对周围状态的引用捆绑在一起，内层函数中访问到其外层函数的作用域

即 **闭包 = 内层函数 + 外层函数的变量**

闭包作用：封闭数据，提供操作，外部也可以访问函数内部的变量

```javascript
// 闭包基本格式
function outer() {
	let a = 1
	function fn() {
		console.log(a)
	}
	return fn
}
const fun = outer()
fun()
// 外部函数可以使用函数内部的的变量,这里fun()就使用的outer()内部的变量a的值
//简约写法：
function outer() {
	let a = 1
	return function fn() {
		console.log(a)
	}
}
```

闭包可以**实现数据的私有**，即外部可以访问函数内部的变量，但是无法直接修改其值；保证了数据安全

```javascript
function count() {
	let i = 0
	function fn() {
		i++
		console.log(`函数被调用了${i}次`)
	}
	return fn
}
const fun = count()		// count()被fun引用，根据标记清除法，count()、fn()、i只有在关闭页面的时候被回收
						// 因此导致了内存泄漏
```

#### **- 变量提升(了解)**

仅存在于var声明变量，它允许在变量声明之前即被访问

```javascript
//var num	 ——>变量提升 
console.log(num + '件')	// undefined件
var num = 10
```

代码在执行之前，先检测当前作用域下所有var声明的变量，把**它们的声明**全部**提升**到当前作用域的最前面

注意：**只提升声明，不提升赋值**，不提倡使用

### 函数进阶

#### 函数提升

代码在执行前，会把所有函数声明提升到当前作用域的最前面，但是不提升函数调用（不提倡，可以忍）

```javascript
fn()
function() {
	//...
}

//函数表达式是赋值，不是声明，fun不是一个函数。所以不可以提升，会报错
fun()
var fun = function() {
    //函数表达式必须先声明和赋值后调用
}
```

#### 函数参数

- **动态参数**

`arguments`是函数内部内置的**伪数组变量**，它包含了调用函数时传入的所有实参

```javascript
function getSum() {
	// arguments 动态参数 只存在于 函数里面
    // 是伪数组 里面存储的是传递过来的实参
    // console.log(arguments)  [2,3,4]
    let sum = 0
    for (let i = 0; i < arguments.length; i++) {
    	sum += arguments[i]
    }
    console.log(sum)
}
getSum(2, 3, 4)
getSum(1, 2, 3, 4, 2, 2, 3, 4)
```

箭头函数中没有arguments

- **剩余参数(更提倡)**

剩余参数允许我们将一个不定数量的参数表示为一个数组

```javascript
function getSum(a, b, ...arr) {
	console.log(arr)  // 使用的时候不需要写 ...
}
getSum(2, 3)			// a=2,b=3,arr为空
getSum(1, 2, 3, 4, 5)	// a=1,b=2,arr=3,4,5
```

...是语法符号，置于最末函数形参之前，用于获取多余的实参

借助...获取的是剩余实参，是个真数组

- **展开运算符**

展开运算符`...`，能将一个数组进行展开

```javascript
const arr = [1,5,3,8,2]
console.log(...arr)
```

不会修改原数组

典型运用场景：求数组最值、合并数组

```javascript
const arr1 = [1, 2, 3]
// 展开运算符 可以展开数组
// console.log(...arr)

// console.log(Math.max(1, 2, 3))
// ...arr1  === 1,2,3
// 1 求数组最大值
console.log(Math.max(...arr1)) // 3
console.log(Math.min(...arr1)) // 1
// 2. 合并数组
const arr2 = [3, 4, 5]
const arr = [...arr1, ...arr2]
console.log(arr)
```

#### 箭头函数

适合于本来需要匿名函数的地方，写法更简洁

**基本语法**

```javascript
// 函数基本写法
const fn = function () {
	console.log(123)
}
————————————————————————————
// 箭头函数基本写法
const fn = () => {
	console.log(123)
}
fn()
————————————————————————————
// 箭头函数带参数
const fn = (x,y) => {
	console.log(x + ' ' + y)
}
fn(5, 20)
————————————————————————————
// 只有一个形参时 可以省略小括号
const fn = x => {
	console.log(x)
}
fn(1)
————————————————————————————
// 只有一行代码时，可以省略大括号
const fn = x => console.log(x)
fn(1)
————————————————————————————
// 只有一行代码的时候，可以省略return
const fn = (x, y) => x + y
console.log(fn(1, 2))
————————————————————————————
// 箭头函数可以直接返回一个对象
const fn = (uname) => ({ uname: uname })
console.log(fn('Lucy'))
```

**参数**

没有动态参数arguments，但是有剩余参数...arr

```javascript
const getSum = (...arr) => {
	let sum = 0
	for(let i = 0; i < arr.length; i++) {
		sum += arr[i]
	}
	return sum
}
const result = getSum(2,3,4)
console.log(result)
```

**箭头函数的this★**

箭头函数不会创建自己的this，所以this指的是自己的**作用域链的上一层**（函数里面才有this）

```javascript
// 1.以前this的指向：  谁调用的这个函数，this 就指向谁
console.log(this)  // window
// 普通函数
function fn() {
    console.log(this)  // window
}
window.fn()

// 对象方法里面的this
const obj = {
    name: 'andy',
    sayHi: function () {
        console.log(this)  // obj
    }
}
obj.sayHi()

// 2.箭头函数的this  是上一层作用域的this 指向
const fn = () => {
    console.log(this)  // window
}
fn()
// 对象方法箭头函数 this
const obj = {
    uname: 'pink老师',
    sayHi: () => {
        console.log(this)  // this 指向谁？ window
    }
    sayHello: function() {
        let i = 10
        const count = () => {
            console.log(this)	// this指向作用域链上一层即function()，普通函数的this指向它的调用者即obj
        }
        count()
    }
}
obj.sayHi()

// 普通函数，此时this指向了DOM对象btn
btn.addEventListener('click', function() {
    console.log(this)
})
// 箭头函数，没有自己的this，此时this指向了window
btn.addEventListener('click', () => {
    console.log(this)
})
// DOM事件回调函数为了简便，不推荐使用箭头函数(this将为全局的window)
```

### 解构赋值

#### 数组解构

数组解构是将数组的单元快速批量赋值给一系列变量的简洁语法

```javascript
const arr = [100, 60, 80]
const [max, min, avg] = arr
console.log(min)

// 交换x y的值
let x = 1
let y = 4;		// 必须加分号
[x, y] = [y, x]
console.log(x, y)
```

**注意：js前面必须加分号的情况**

- 多个立即执行函数之间

  ```
  (function t() {})();
  或者
  ;(function t() {})()
  ```

- 数组解构

  ```javascript
  // 如果前面有代码，还以数组作为一行的开头，会出错，需要用分号隔开
  ;[x, y] = [y, x]
  ```

**1.变量多、单元值少的情况——**

```javascript
const [a,b,c,d] = [1,2,3]
//1,2,3,undefined
```

**2.变量少，单元值多的情况——**

```javascript
const [a,b] = [1,2,3]
//1,2
```

**3.剩余参数 变量少，单元值多的情况——**

```javascript
const [a,b,...arr] = [1,2,3,4]	// 剩余参数只能放在末尾
//1,2,[3,4]真数组
```

**4.可以"设置默认值" 来防止有undefined传递单元值的情况——**

```javascript
const [a = 0, b = 0] = [1, 2]	//1,2
cosnt [a = 0, b = 0] = []		//0,0
```

**5.按需导入，忽略某些返回值**

```javascript
// 看见了，假装没看见
const [a, , c] = ['小米', '苹果', '华为']
```

**6.支持多维数组的解构**

```javascript
const arr = [1,2,[3,4]]
console.log(arr[2][0])	// 3
```

```javascript
const [a,b,[c,d]] = [1,2,[3,4]]
console.log(d)		// 4
```

#### 对象解构★

对象解构是将对象的属性和方法快速批量赋值给一系列变量的简洁语法

```javascript
const obj = {
	uname: 'pink老师',
	age: 18,
};

const {uname, age} = obj	// uname = obj.uname, age = uname.age
```

- 对象的属性名要和变量名必须一致
- 注意解构的变量名不要与外面的变量名冲突，否则报错
- 对象中找不到与变量名一致的属性时变量值为undefined

**对象解构的变量名 可以重新改名 ——旧变量名: 新变量名**

```javascript
const { uname: username, age } = { uname: 'pink老师', age: 18 }

console.log(username)

console.log(age)
```

**解构数组对象**

```javascript
const pig = [
	{
		name: '佩奇',
		'age' 6
	}
]

const [{name, age}] = pig
console.log(name. age)
```

**多级对象解构**

```javascript
const pig = {
	name: '佩奇',
	family: {
		mother: '猪妈妈',
    	father: '猪爸爸',
        sister: '乔治'
	}
	age: 6
}

const { name, family: { mother, father, sister } } = pig
console.log(sister)
```

```javascript
const person = [
    {
        name: '佩奇',
        family: {
            mother: '猪妈妈',
            father: '猪爸爸',
            sister: '乔治'
    	},
    	age: 6
    }
]
const [{ name, family: { mother, father, sister } }] = person
console.log(mother)
```

案例：获取json中的data数据并更名为myDate

```javascript
// 这是后台传递过来的数据
const msg = {
    "code": 200,
    "msg": "获取新闻列表成功",
    "data": [
        {
            "id": 1,
            "title": "5G商用自己，三大运用商收入下降",
            "count": 58
        },
        {...},
        {...},
    ]
}

function render({ data: myData }) {
      // 内部处理
      console.log(myData)
    }
render(msg)
```

### 综合案例——价格筛选

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>商品渲染</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    .list {
      width: 990px;
      margin: 0 auto;
      display: flex;
      flex-wrap: wrap;
    }

    .item {
      width: 240px;
      margin-left: 10px;
      padding: 20px 30px;
      transition: all .5s;
      margin-bottom: 20px;
    }

    .item:nth-child(4n) {
      margin-left: 0;
    }

    .item:hover {
      box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.2);
      transform: translate3d(0, -4px, 0);
      cursor: pointer;
    }

    .item img {
      width: 100%;
    }

    .item .name {
      font-size: 18px;
      margin-bottom: 10px;
      color: #666;
    }

    .item .price {
      font-size: 22px;
      color: firebrick;
    }

    .item .price::before {
      content: "¥";
      font-size: 14px;
    }

    .filter {
      display: flex;
      width: 990px;
      margin: 0 auto;
      padding: 50px 30px;
    }

    .filter a {
      padding: 10px 20px;
      background: #f5f5f5;
      color: #666;
      text-decoration: none;
      margin-right: 20px;
    }

    .filter a:active,
    .filter a:focus {
      background: #05943c;
      color: #fff;
    }
  </style>
</head>

<body>
  <div class="filter">
    <a data-index="1" href="javascript:;">0-100元</a>
    <a data-index="2" href="javascript:;">100-300元</a>
    <a data-index="3" href="javascript:;">300元以上</a>
    <a href="javascript:;">全部区间</a>
  </div>
  <div class="list">
    <!-- <div class="item">
      <img src="" alt="">
      <p class="name"></p>
      <p class="price"></p>
    </div> -->
  </div>
  <script>
    // 2. 初始化数据
    const goodsList = [
      {
        id: '4001172',
        name: '称心如意手摇咖啡磨豆机咖啡豆研磨机',
        price: '289.00',
        picture: 'https://yanxuan-item.nosdn.127.net/84a59ff9c58a77032564e61f716846d6.jpg',
      },
      {
        id: '4001594',
        name: '日式黑陶功夫茶组双侧把茶具礼盒装',
        price: '288.00',
        picture: 'https://yanxuan-item.nosdn.127.net/3346b7b92f9563c7a7e24c7ead883f18.jpg',
      },
      {
        id: '4001009',
        name: '竹制干泡茶盘正方形沥水茶台品茶盘',
        price: '109.00',
        picture: 'https://yanxuan-item.nosdn.127.net/2d942d6bc94f1e230763e1a5a3b379e1.png',
      },
      {
        id: '4001874',
        name: '古法温酒汝瓷酒具套装白酒杯莲花温酒器',
        price: '488.00',
        picture: 'https://yanxuan-item.nosdn.127.net/44e51622800e4fceb6bee8e616da85fd.png',
      },
      {
        id: '4001649',
        name: '大师监制龙泉青瓷茶叶罐',
        price: '139.00',
        picture: 'https://yanxuan-item.nosdn.127.net/4356c9fc150753775fe56b465314f1eb.png',
      },
      {
        id: '3997185',
        name: '与众不同的口感汝瓷白酒杯套组1壶4杯',
        price: '108.00',
        picture: 'https://yanxuan-item.nosdn.127.net/8e21c794dfd3a4e8573273ddae50bce2.jpg',
      },
      {
        id: '3997403',
        name: '手工吹制更厚实白酒杯壶套装6壶6杯',
        price: '100.00',
        picture: 'https://yanxuan-item.nosdn.127.net/af2371a65f60bce152a61fc22745ff3f.jpg',
      },
      {
        id: '3998274',
        name: '德国百年工艺高端水晶玻璃红酒杯2支装',
        price: '139.00',
        picture: 'https://yanxuan-item.nosdn.127.net/8896b897b3ec6639bbd1134d66b9715c.jpg',
      },
    ]

    // 1. 渲染函数  封装
    function render(arr) {
      // 声明空字符串
      let str = ''
      // 遍历数组 
      arr.forEach(item => {
        // 解构
        const { name, picture, price } = item
        str += `
         <div class="item">
          <img src=${picture} alt="">
          <p class="name">${name}</p>
          <p class="price">${price}</p>
        </div> 
        `
      })
      // 追加给list 
      document.querySelector('.list').innerHTML = str
    }
    render(goodsList)  // 页面一打开就需要渲染

    // 2. 过滤筛选  
    document.querySelector('.filter').addEventListener('click', e => {
      // e.target.dataset.index   e.target.tagName
      const { tagName, dataset } = e.target
      // 判断 
      if (tagName === 'A') {
        // console.log(11) 
        // arr 返回的新数组
        let arr = goodsList		
        //★index = 4为全部区间，所以如果index不是1,2,3那么就一定是4，则arr直接为goodsList，即全部商品
        if (dataset.index === '1') {
          arr = goodsList.filter(item => item.price > 0 && item.price <= 100)
        } else if (dataset.index === '2') {
          arr = goodsList.filter(item => item.price >= 100 && item.price <= 300)
        } else if (dataset.index === '3') {
          arr = goodsList.filter(item => item.price >= 300)
        }
        // 渲染函数
        render(arr)
      }
    })
  </script>
</body>

</html>
```



## day2-构造函数&数据常用函数

### 深入对象

#### 创建对象三种方式

- 1.利用对象字面量创建对象

```javascript
const o = {
	name: '佩奇'
}
```

- 2.利用new Object创建对象

```javascript
const o = new Object {{ name: '佩奇' }}
console.log(o)	// {name: '佩奇'}
```

- 3.利用构造函数创建对象↓

#### 构造函数

可以利用构造函数来**快速创建多个类似的对象**

约定：

- 命名以大写字母开头
- 只能由`new`关键字来执行

说明：

- 使用new关键字调用函数的行为被称为实例化
- 实例化构造函数时没有参数时可以省略()
- 构造函数内部无需写return，返回值即为新创建的对象；写了return返回的值也是无效的
- new Object()、new Date()也是实例化构造函数

```javascript
function Goods(name, price, count) {
    this.name = name			// 左边为属性，右边为形参
    this.price = price
    this.count = count
}

const mi = new Goods('小米', 1999, 20)
console.log(mi)
const hw = new Goods('华为', 3999, 50)
console.log(hw)
```

实例化执行过程

1. 创建新对象——空对象
2. 构造函数this指向新对象
3. 执行构造函数代码，修改this，添加新的属性
4. 返回新对象——含有数据

#### 实例成员&静态成员

通过构造函数创建的对象为实例对象，实例对象中的属性和方法称为**实例成员**（实例属性和实例方法）

```javascript
const mi = new Goods('小米', 1999, 20)
mi.name = 'vivo'	// mi是实例对象，它的属性name和方法intro()是实例成员
mi.intro = function () { }
```

构造函数创建的实例对象**彼此独立**互不影响

————————

构造函数中的属性和方法被称为**静态成员**（静态属性和静态方法）

静态成员只能构造函数来访问

静态方法中的this指向构造函数

比如：Date.now()、Math.PI、Math.random()

```javascript
Goods.num = 10					// Goods是构造函数，它的属性num和方法sayhi()是静态成员
Goods.sayhi = function () { }
```

### 内置构造函数

JS中主要的数据类型有6种——基本数据类型：字符串、数值、布尔、undefined、null；引用类型：对象

```javascript
const str = 'pink'
// js底层完成，把简单数据类型包装为了引用数据类型
const str = new String('pink')
```

字符串、数值、布尔等基本数据类型也有专门的构造函数，我们称为**包装类型**

JS中几乎所有数据都可以基于构造函数创建



- 引用类型
  - Object，Array，RegExp，Date等
- 包装类型
  - String，Number，Boolean等



#### Object

**Object是内置构造函数，用于创建普通对象**

```javascript
const user = new Object({name: '小明', age: 15})
```

当然，推荐使用字面量方式声明对象，而不是Object构造函数

- **Object.keys()、Object.values()静态方法**——获取对象中所有属性(键)，返回一个数组

```javascript
const o = { name: '佩奇', age: 6}
// 获取对象中所有键，并且返回的是一个数组
console.log(Object.keys(o))
console.log(Object.values(o))

旧方法：
for (let k in o) {
    console.log(k)		// 属性
    console.log(o[k])	// 值
}
```

- **Object.assign()静态方法**——常用于对象追加式拷贝（有的就覆盖，没有的就追加）

```javascript
const o = { name: '佩奇', age: 6}
const obj = {}
Object.assign(obj, o)	// 把对象o拷贝给对象obj
console.log(obj)
```

使用场景：给对象追加属性

```javascript
const o = { name: '佩奇', age: 6}
Object.assign(o, {gender: '女'})
console.log(o)			// { name: '佩奇', age: 6, gender: '女'}
```

#### Array

**Array是内置构造函数，用于创建数组**

```javascript
const arr = new Array(3,5)
```

建议使用字面量创建，不用Array构造函数创建

一、数组常见**核心实例方法**：

| 方法    | 作用     | 说明                                               |
| ------- | -------- | -------------------------------------------------- |
| forEach | 遍历数组 | 不返回数组，经常用于**查找遍历**数组元素           |
| filter  | 过滤数组 | **返回新数组**，返回的是**满足筛选条件**的数组元素 |
| map     | 迭代数组 | **返回新数组**，返回的是**处理后**的数组元素       |
| reduce  | 累计器   | 返回累计处理的结果，经常用于求和等                 |

- reduce()累计器

```javascript
arr.reduce(function(){}, 起始值)
arr.reduce(function(上一次值，当前值){}，初始值)
```

```javascript
const arr = [1, 5, 8]

const num1 = arr.reduce((prev, current) => prev + current)		// 14
const num2 = arr.reduce((prev, current) => prev + current, 10)	// 24
```

计算薪资案例：

```javascript
const arr = [{
    name: '张三',
    salary: 10000
},
    ...
]
const money = arr.reduce((prev, item) => prev + item.salary * 1.3, 0)
console.log(money)
```

二、数组常见**其他实例方法**：

join——数组元素拼接为字符串，返回字符串**(重点)**

find——查找元素，返回符合测试条件的第一个数组元素值，如果没有符合条件的则返回undefined**(重点)**

```javascript
// find查找的价值在于可以返回第一个符合测试条件  的对象
const mi = arr.find(item => item.name === '小米')
console.log(mi)
```

every——检测数组所有元素**是否都符合指定条件**，如果所有元素都通过检测返回true，否则返回false**(重点)**

some——检测数组中的元素是否满足指定条件**如果数组中有元素满足条件返回true**，否则返回false

concat——合并两个数组，返回生成新数组

sort——对原数组单元值排序

splice——删除或替换原数组单元

reverse——反转数组

findIndex——查找元素的索引值

**——有问题上mdn查**

**静态方法Array.from()**——伪数组转换为真数组

伪数组没有pop等方法，需要转为真数组



#### String

length——用来获取字符串的度长(重点)

`split( '分隔符')`——用来将字符串拆分成数组(重点)；join('连接符')方法与之相反

`substring (开始的索引号[,结束的索引号])`——截取索引号之间的字符串(重点)

`startswith(检测字符串[，检测位置索引号])`——检测是否以某字符开头(重点)

```javascript
let str = 'To be, or not to be, that is the question.'

alert(str.startswith("not to be", 10));		// true
```

`endswith`——检测是否以某字符结尾

`includes(搜索的字符串[，检测位置索引号])`——判断一个字符串是否包含在另一个字符串中，根据情况返回true或 false(重点)

toUppercase——用于将字母转换成大写

toLowercase——用于将就转换成小写

indexof——检测是否包含某字符

replace——用于替换字符串，支持正则匹配

match——用于查找字符串，支持正则匹配



#### Number

**Number是内置构造函数，用来创建数值**

toFixed()——设置保留小数位的长度

```javascript
const price = 12.345
const num = 10
console.log(price.toFixed(2))	// 12.34
console.log(num.toFixed(2))	// 10.00
```



### 综合案例(待完成)

**购物车案例**（待完成）



## day3-深入面向对象

### 编程思想

面向过程——分析问题所需的解决步骤，用函数一步步实现，一个个依次调用函数

**面向对象**——把事务分解成为一个个对象，由对象之间分工与合作。即以对象功能来划分问题，而不是步骤

- 特性：封装性、继承性、多态性

面向过程性能更高，但是不灵活，复用性差；	面向对象更易维护、复用和扩展，但是性能不好



**构造函数**

构造函数体现了面向对象的封装性；构造函数实例创建的对象彼此独立、互不影响；

也存在浪费内存的问题，构造函数中的函数会多次创建，即使一模一样的代码也会分别开辟内存

原型——可以解决构造函数浪费内存的问题，即把函数单独存储起来，写在原型对象中，所有实例对象共享



### 原型对象prototype

- 构造函数通过prototype属性分配的函数是所有对象所 **共享的**。

- JavaScript 规定，**每一个构造函数都有一个 prototype 属性**，指向原型对象
- 原型对象可以挂载函数，对象实例化不会多次创建原型对象上的函数，可以节约内存
- 把内容不变的方法，直接定义在 prototype 对象上，这样所有对象的实例就可以共享这些方法

```javascript
function Star(uname, age) {
	this.uname = uname
	this.age = age
}
Star.prototype.sing = function() {
	console.log('唱歌')
}
const ldh = new Star('刘德华',55)
const zxy = new Star('张学友', 58)

ldh.sing()
zxy.sing()
	
console.log(ldh.sing === zxy.sing)	// true
```

*公共属性写在构造函数里，公共方法写在原型对象上*

- **构造函数的this** 和 **原型对象(函数)中的this** 都指向 **实例化的对象**

```javascript
let that
function Star(uname) {
    // that = this
    // console.log(this)
    this.uname = uname
}
// 原型对象里面的函数this指向的还是 实例对象 ldh
Star.prototype.sing = function () {
    that = this
    console.log('唱歌')
}
// 实例对象 ldh   
// 构造函数里面的 this 就是  实例对象  ldh
const ldh = new Star('刘德华')
ldh.sing()
console.log(that === ldh)
```

小例子——给Array添加求最大值和求和方法

```javascript
const arr = [1,2,3]

Array.prototype.max = function() {
    return Math.max(...this)
}
Array.prototype.sum = function() {
    return this.reduce((prev, item) => prev + item, 0)
}

console.log(arr.max())
console.log([2,5,3].max())
console.log([1,2,3].sum())
```

### constructor属性

每个构造函数默认有一个原型对象；每个原型对象里默认有一个constructor属性，指向该原型对象的构造函数（指回爸爸）

原型对象里面的**constructor属性**的**作用**是**知道创建自己的是哪个构造函数**。如果没有constructor属性，这个原型对象就是孤儿

```javascript
function Star() { }
console.log(Star.prototype)
Star.prototype = {
    // constructor: Star,      // 重新指回创建这个原型对象的构造函数
    sing: function() {
        console.log('唱歌')
    },
    dance: function() {
        console.log('跳舞')
    }
}
console.log(Star.prototype)
```



### 对象原型`__proto__`

每一个对象都有一个属性`__proto__`指向构造函数的prototype原型对象，因此实例对象可以使用构造函数的prototype原型对象的属性和方法

- `__proto__`是JS非标准属性，标准写法是[[prototype]]，意义相同
- 表明当前实例对象指向哪个原型对象prototype
- `__proto_`里面也有一个constructor，指向创建该实例对象的构造函数

```javascript
function Star() { }
const ldh = new Star()
console.log(ldh.__proto__ === Star.prototype)	// true
console.log(ldh.__proto__.constructor === Star)	// true
```

<img src="前端学习笔记★.assets/image-20230917171128569.png" alt="image-20230917171128569" style="zoom: 67%;" />



### 原型继承

JS中大多借助原型对象实现继承的特性

原型继承，即通过原型来继承

```javascript
const Person = {
    eyes: 2,
    head: 1
}
function Man(){ }
Man.prototype = Person
Man.prototype.constructor = Man
const ldh = new Man()
console.log(ldh.eyes)
console.log(Man.prototype)
```

遇到问题：把Person对象挂载到Woman和Man构造函数上。如果给Woman的prototype添加baby方法，则其实是给Person对象添加baby方法，会出现Man的原型对象也会有baby方法

解决方法：原型对象用构造函数

——把构造函数Person挂载到Woman和Man构造函数上。Woman和Man的原型对象都是基于构造函数Person创建的对象，所以Woman和Man的原型对象相同，但是相互独立互不影响，此时给Woman的原型对象添加baby方法不会影响Man的原型对象

```javascript
function Person() {
    this.eyes = 2
    this.head = 1
}

function Woman() { }
Woman.prototype = new Person()
Woman.prototype.constructor = Woman
Woman.prototype.baby = function () {
	console.log('宝贝')
}
const xiaohua = new Woman()
console.log(red)

function Man() { }
Man.prototype = new Person()
Man.prototype.constructor = Man
const pink = new Man()
console.log(pink)
```



### 原型链

`Object.prototype.__proto__` **=>** `null`

`Person.prototype.__proto__`**=>** `Object.prototype`

`ldh.__proto__ `**=>** `Person.prototype` 

基于原型对象的继承使得不同构造函数的原型对象关联在一起，并且呈现一种链状的结构，我们将其称为**原型链**

通俗来讲：一层一层往上，每层都是一个（构造函数，原型对象，实例对象）的三角，将三角串起来的就是原型链

- 只要是对象，就有`__proto__`
- 只要是原型对象，就有`constructor`

原型链——查找规则（查找链）：

1. 当访问一个对象的属性(包括方法)时，首先查找这个对象自身有没有该属性。
2. 如果没有就查找它的原型 (也就是 _proto 指向的 prototype 原型对象)
3. 如果还没有就查找原型对象的原型 (object的原型对象)
4. 依此类推一直找到 object 为止 (null)
5. `__proto__` 对象原型的意义就在于为对象成员查找机制提供一个方向，或者说一条路线
6. 可以使用 instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上

```javascript
console.log(ldh instanceof Person)	// true 检测构造函数Person的prototype 是否出现在ldh实例对象的原型链上
console.log(ldh instanceof Object)	// true
console.log(ldh instanceof Array)	// false
console.log(Array instanceof Object)// true
```



### 综合案例—消息框

消息提示框对象封装

```javascript
function Modal(title = '', message = '') {
    this.title = title
    this.message = message
    this.modalBox = document.createElement('div')
    this.modalBox.className = 'modal'
    this.modalBox.innerHTML = `
        <div class="header">${this.title} <i>x</i></div>
        <div class="body">${this.message}</div>
    `
}

Modal.prototype.open = function() {				// 需要用到this，所以这里不用箭头函数
    if(!document.querySelector('.modal')){
        document.body.appendChild(this.modalBox)
        this.modalBox.querySelector('i').addEventListener('click', () => {
            this.close()
        })
    }
}

Modal.prototype.close =function() {
	document.body.removeChild(this.modalBox)
}

document.querySelector('#delete').addEventListener('click', () => {
    const m = new Modal('温馨提示', '您没有权限删除')
    m.open()
})

document.querySelector('#login').addEventListener('click', () => {
    const m = new Modal('友情提示', '您还没有注册账号')
    m.open()
})
```



## day4-高阶技巧

### 深浅拷贝

深浅拷贝**只针对引用类型，即Array、Object等**

直接赋值：赋值地址，直接赋值的对象任何修改都能影响原对象

- 浅拷贝：**简单数据类型**直接拷贝**值**；**引用数据类型**拷贝的是**地址**（即如果是多层对象会出现问题） 

```javascript
const obj = {
    uname: 'pink',
    age: 18,
    family: {
    	baby: '小pink'
    }
}
const o = {}
Object.assign(o, obj)
o.age = 20
o.family.baby = '老pink'	
console.log(o)		// age = 20, family: {baby: '老pink'}
console.log(obj)	// age = 18, family: {baby: '老pink'}	多层对象是引用数据类型，拷贝地址，一改全改
```

 

- 深拷贝：**简单数据类型**直接拷贝**值**；**引用数据类型**拷贝的是对象
  - 通过递归实现
  - lodash/cloneDeep
  - 通过JSON.stringify()实现









### 异常处理



### 处理this



### 性能优化



### 综合案例

