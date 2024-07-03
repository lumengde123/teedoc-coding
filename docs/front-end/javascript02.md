---
title: Web APIs--DOM,BOM
keywords: JavaScript
desc: JavaScript学习笔记
date: 2024-06-16
class: heading_no_counter
tags: 笔记, JavaScript
---


## day1：DOM-获取元素

**声明变量优先选 const**

const 优先，尽量用const，其次再考虑let，var排除。`可以变量先用const，需要改变值了再改为let`

- 建议数组和对象使用 const 来声明

```javascript
由上一小节数据类型存储注意点的内容可知
let person = {
    uname: 'pink',
    age: 18,
    gender: 'male'
}
person.address = '武汉黑马'
console.log(person)
// 这里的let可以用const替换，因为person是个对象，修改person的内容是修改"堆"里面的内容，
// 与person在栈中存的内容（person在堆中的首地址）无关

const arr = ['red', 'blue']
arr = [1, 2, 3]
console.log(arr)    // 此时会出错，相当于arr指向了另一块堆，这个堆空间由另一个栈空间指向，			 
                    // 所以arr指向的栈空间发生了改变，因此不能用const，出错了
```


### Web API 基本认知

作用：使用 JS 去操作 html 和浏览器

分类：DOM（文档对象模型）、BOM（浏览器对象模型）

**DOM**

- 操作网页内容，可以开发网页内容特效和实现用户交互

**DOM树**

- 将 HTML 文档以树状结构直观的表现出来（体现html中 标签与标签的关系）

**DOM对象**

- 浏览器根据 HTML 标签生成的 **JS对象**
- `所有的标签属性都可以在这个对象上找到`
- 修改这个对象的属性会自动映射到标签身上

**DOM核心思想**

- 把网页内容当作对象来处理

**document对象**

- 是 DOM 里提供的一个对象
- 它提供的属性和方法都是用来**访问和操作网页内容**的
- `网页所有内容都在 document 里面`

### 获取DOM对象

**根据CSS选择器来获取DOM元素（重点）**

1.选择匹配的第一个元素

```javascript
document.querySelector('css选择器')
```

```javascript
const div = document.querySelector('div')
const box = document.querySelector('.box')
const nav = document.querySelector('#nav')
const li = document.querySelector('ul li:first-child')

// 返回值是一个HTMLElement元素；如果没有匹配到则返回null
```

获取的单个对象可以直接修改样式

```javascript
const nav = document.querySelector('#nav')
nav.style.color = 'red'
```

2.选择匹配的多个元素

```javascript
document.querySelectorAll('css选择器')	// 得到的是一个伪数组：有长度有索引，但是没有pop()和push()
                                        // 即使只有一个元素也会是数组
```

```javascript
const lis = document.querySelectorAll('.nav li')// 获取所有的li,返回值是一个NodeList对象集合(数组)

for(let i = 0; i < lis.length; i++) {	// 获得集合中的每个对象
    console.log(lis[i])
}
```

**其他获取DOM元素方法（了解，不再使用）**

```javascript
document.getElementById('nav')
document.getElementByTagName('div')
document.getElementByClassName('w')
```

### 操作元素内容

**1.元素inerText属性**

- 将文本内容添加/更新到任意标签位置
- 只显示纯文本，`不解析标签`

```javascript
const box = document.querySelector('.box')
console.log(box.innerText)
box.innerText = '<strong>我是一个盒子</strong>'	// 不解析标签，所以文字不加粗，而是显示纯文本
```

**2.元素innerHTML属性**

- 将文本内容添加/更新到任意标签位置
- `会解析标签`，多标签建议使用模板字符

```javascript
console.log(box.innerHTML)
// box.innerHTML = '我要更换'
box.innerHTML = '<strong>我要更换</strong>'
```

**3.操作元素属性**

- 操作元素常用属性

  ```javascript
  const pic = document.querySelector('.pic')
  pic.src = './images/lion.webp'
  pic.width = 400;
  pic.alt = '图片不见了...'
  ```

- 操作元素样式属性

  - 通过行内样式 `style` 属性修改样式

    ```javascript
    const box = document.querySelector('.box')
    box.style.width = '300px'
    box.style.backgroundColor = 'hotpink'	// 多组单词的（含有符号：-），采用小驼峰命名法
    box.style.border = '2px solid blue'
    box.style.borderTop = '2px solid red'
    // 注意，行内样式的权重更高
    ```

  - 通过类名 `className` 修改样式

    ```javascript
    .box {
        //...
    }
    
    // 给 div 标签添上一个类名 box，直接获得box类的所有样式
    const div = document.querySelector('div')
    div.className = 'box'	// 特点：会直接覆盖以前的类名，新值换旧值
    ```

  - 通过`classList`修改样式

    ```javascript
    const box = document.querySelector('.box')
    box.classList.add('active')     // 追加
    box.classList.remove('active')	// 删除
    box.classList.toggle('active')	// 切换：有就删除，没有就追加
    box.classList.contains('active')// 包含active类返回true，没有则返回false
    
    // 注意，里面只要填类名，不用加点
    ```

- **操作表单元素属性**

  ```javascript
  <input type="text" value="电脑">
  
  
  const uname = document.querySelector('input')
  
  console.log(uname.value) 	// 电脑
  // console.log(uname.innerHTML)  innertHTML 得不到表单的内容
  
  uname.value = '我要买电脑'
  console.log(uname.type)		//type为text
  uname.type = 'password'
  ```

  - 表单属性中，添加就有效果，移除就没有效果，**只接受布尔值表**。true为添加，false为移除该属性
  - 例如：checked，disabled，selected

  ```javascript
  <input type="checkbox" name="" id="">
      
  // 1. 获取
  const ipt = document.querySelector('input')
  console.log(ipt.checked)  // 默认false，表示没有勾选
  ipt.checked = true
  
  // ipt.checked = 'true'  // 会选中，不提倡  有隐式转换
  ```

  ```javascript
  <button>点击</button>
  
  const button = document.querySelector('button')
  // console.log(button.disabled)  // 默认false 不禁用
  button.disabled = true   // 禁用按钮
  
  // <button disabled>点击</button>
  ```

- 自定义属性

  - html5 新推出的专门的 `data-自定义属性`
  - 标签上一律以 `data-` 开头
  - 在DOM对象上一律以 `dataset` 对象方式获取自定义属性

  ```javascript
  <div data-id="1" data-spm="不知道">1</div>
  
  const one = document.querySelector('div')
  console.log(one.dataset.id)		// 1
  console.log(one.dataset.spm)	// 不知道
  ```

**标准属性**：标签天生自带的属性(class id title...)，可以直接使用点语法操作(checked disabled selected)

### 定时器-间歇函数

定时器函数可以开启和关闭定时器

```javascript
// 开启定时器
setInterval(匿名函数/函数名, 间隔时间)		// 间隔时间单位是毫秒，返回值是该定时器的id。
```

```javascript
setInterval(function() {
    console.log('一秒执行一次')
}, 1000)
```

```javascript
function fn() {
    console.log('一秒执行一次')
}

setInterval(fn, 1000)	// 这里不要加()，不然只会调用一次。要让它自动调用
```

关闭定时器

```javascript
let timer = setInterval(function() {    // 开关定时器会赋新值(新的定时器，新的id)，所以用let
    console.log('hi~')
}, 1000)

clearInterval(timer)
```

**案例：用户注册倒计时按钮**

```html
<textarea name="" id="" cols="30" rows="10">
    用户注册协议
    欢迎注册成为京东用户！在您注册过程中，您需要完成我们的注册流程并通过点击同意的形式在线签署以下协议，请您务必仔细阅读、充分理解协议中的条款内容后再点击同意（尤其是以粗体或下划线标识的条款，因为这些条款可能会明确您应履行的义务或对您的权利有所限制）。
    【请您注意】如果您不同意以下协议全部或任何条款约定，请您停止注册。您停止注册后将仅可以浏览我们的商品信息但无法享受我们的产品或服务。如您按照注册流程提示填写信息，阅读并点击同意上述协议且完成全部注册流程后，即表示您已充分阅读、理解并接受协议的全部内容，并表明您同意我们可以依据协议内容来处理您的个人信息，并同意我们将您的订单信息共享给为完成此订单所必须的第三方合作方（详情查看
</textarea>
<br>
<button class="btn" disabled>我已经阅读用户协议(5)</button>
```

```javascript
// 1. 获取元素
const btn = document.querySelector('.btn')
// console.log(btn.innerHTML)  butto按钮特殊，要用innerHTML	--对象.value 用于单标签
// 2. 倒计时
let i = 5
// 2.1 开启定时器
let n = setInterval(function () {   // 返回值n为该定时器的编号，可用来选中该定时器
    i--
    btn.innerHTML = `我已经阅读用户协议(${i})`
    if (i === 0) {
        clearInterval(n)  // 关闭定时器
        // 定时器停了，我就可以开按钮
        btn.disabled = false
        btn.innerHTML = '同意'
    }
}, 1000)
```

**案例：轮播图定时切换**

```html
<div class="slider">
    <div class="slider-wrapper">
        <img src="./images/slider01.jpg" alt="" />
    </div>
    <div class="slider-footer">
        <p>对人类来说会不会太超前了？</p>
        <ul class="slider-indicator">
        <li class="active"></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        </ul>
        <div class="toggle">
        <button class="prev">&lt;</button>
        <button class="next">&gt;</button>
        </div>
    </div>
</div>
```

```javascript
// 1. 初始数据
const sliderData = [
    { url: './images/slider01.jpg', title: '对人类来说会不会太超前了？', color: 'rgb(100, 67, 68)' },
    { url: './images/slider02.jpg', title: '开启剑与雪的黑暗传说！', color: 'rgb(43, 35, 26)' },
    { url: './images/slider03.jpg', title: '真正的jo厨出现了！', color: 'rgb(36, 31, 33)' },
    { url: './images/slider04.jpg', title: '李玉刚：让世界通过B站看到东方大国文化', color: 'rgb(139, 98, 66)' },
    { url: './images/slider05.jpg', title: '快来分享你的寒假日常吧~', color: 'rgb(67, 90, 92)' },
    { url: './images/slider06.jpg', title: '哔哩哔哩小年YEAH', color: 'rgb(166, 131, 143)' },
    { url: './images/slider07.jpg', title: '一站式解决你的电脑配置问题！！！', color: 'rgb(53, 29, 25)' },
    { url: './images/slider08.jpg', title: '谁不想和小猫咪贴贴呢！', color: 'rgb(99, 72, 114)' },
]
// 1. 获取元素 
const img = document.querySelector('.slider-wrapper img')
const p = document.querySelector('.slider-footer p')
let i = 0  // 信号量 控制图片的张数
// 2. 开启定时器
// console.log(sliderData[i])  拿到对应的对象啦
setInterval(function () {
    i++
    // 无缝衔接位置  一共八张图片，到了最后一张就是 8， 数组的长度就是 8
    if (i >= sliderData.length) {
        i = 0
    }
    // console.log(i)
    // console.log(sliderData[i])
    // 更换图片路径  
    img.src = sliderData[i].url
    // 把字写到 p里面
    p.innerHTML = sliderData[i].title
    // 小圆点
    // 先删除以前的active
    document.querySelector('.slider-indicator .active').classList.remove('active')
    // 只让当前li添加active
    document.querySelector(`.slider-indicator li:nth-child(${i + 1})`).classList.add('active')
}, 1000)
```

## day2：DOM-事件基础

### 事件监听

- **事件源**：哪个DOM元素被事件触发了，要获取这个DOM元素
- **事件类型**：用什么方式触发，比如鼠标单击click，鼠标经过mouseover等
- **事件调用的函数**：要做什么事

```
元素对象.addEventListener('事件类型', 要执行的函数)	// 每次触发，每次执行
```

```javascript
const btn = document.querySelector('.btn')
btn.addEventListener('click', function() {
    alert('你好呀')
})
```

```javascript
// 关闭盒子功能
// 1. 获取事件源
const box = document.querySelector('.box')
// 2. 事件侦听
box.addEventListener('click', function () {
    box.style.display = 'none'
})
```

**案例：随机点名案例**
qs中显示当前名字，start和end为开始和关闭按钮

```javascript
// 数据数组
const arr = ['马超', '黄忠', '赵云', '关羽', '张飞']
// 定时器的全局变量
let timerId = 0
// 随机号要全局变量
let random = 0
// 业务1.开始按钮模块
const qs = document.querySelector('.qs')
// 1.1 获取开始按钮对象
const start = document.querySelector('.start')
// 1.2 添加点击事件
start.addEventListener('click', function () {
    timerId = setInterval(function () {
        // 随机数
        random = parseInt(Math.random() * arr.length)
        // console.log(arr[random])
        qs.innerHTML = arr[random]
    }, 35)
    // 如果数组里面只有一个值了，还需要抽取吗？  不需要  让两个按钮禁用就可以
    if (arr.length === 1) {
        // start.disabled = true
        // end.disabled = true
        start.disabled = end.disabled = true
    }
})

// 业务2.关闭按钮模块
const end = document.querySelector('.end')
end.addEventListener('click', function () {
    clearInterval(timerId)
    // 结束了，可以删除掉当前抽取的那个数组元素
    arr.splice(random, 1)
    console.log(arr)
})
```

补充知识点：点击事件内，函数内的const常量，当我们执行完这次事件/函数，这个常量不再使用，它会被自动销毁/回收掉（这里用到了垃圾回收机制），下次再点击的时候会创建一个新的同名常量，**所以它可以用const而不用let，不会冲突**



- 事件监听历史版本：`事件源.on事件 = function(){}`，例如：btn.onclick(){...}
- 区别：on方式后者会覆盖前者，但addEventListener方式可以绑定多次，拥有事件更多特性

### 事件类型

**1.鼠标事件**：click，mouseenter，mouseleave

案例：轮播图完整版 day02-08

**2.焦点事件**：focus，blur

案例：点击文本框显示下拉菜单

- 失去焦点 `display:none`，获得焦点 `display:block`
- 文本框获得焦点改变边框颜色，单独写 一个css样式，add这个类；失去焦点就remove它

**3.键盘事件**：Keydown，Keyup

**4.文本事件**：input

一旦在文本框内输入，就触发

**5.change事件**

当鼠标离开了表单，并且表单值发生了变化时触发

**案例：用户当前输入字数统计 day02-13**

```javascript
const tx = document.querySelector('#tx')
const total = document.querySelector('.total')
// 1. 当我们文本域获得了焦点，就让 total 显示出来
tx.addEventListener('focus', function () {
    total.style.opacity = 1
})
// 2. 当我们文本域失去了焦点，就让 total 隐藏出来
tx.addEventListener('blur', function () {
    total.style.opacity = 0
})
// 3. 检测用户输入
tx.addEventListener('input', function () {
    // console.log(tx.value.length)  得到输入的长度
    total.innerHTML = `${tx.value.length}/200字`
})
```

### 事件对象

**获取事件对象**

事件对象是个对象，存了事件发生时的相关信息。例如：鼠标点击事件中，事件对象存了鼠标点击在哪个位置等信息

在事件绑定的回调函数的第一个参数就是事件对象，一般命名为event、ev、e

```javascript
元素.addEventListener('click', function(e) {

})
```

**事件对象常用属性**

- type：获取当前的事件类型

- clientX/clientY：获取光标相对于浏览器可见窗口左上角的位置

- offsetX/offsetY：获取光标相对于当前DOM元素左上角的位置

- key：用户按下的键盘键的值。ketcode已废弃，不要用

  ```javascript
  const input = document.querySelector('input')
  input.addEventListener('keyup', function(e) {
      if(e.key === 'Enter') {
          console.log('我按下了回车键')
      }
  })
  ```

**案例：按下回车发布评论 day02-15**

```javascript
// 4. 按下回车发布评论
tx.addEventListener('keyup', function (e) {
    // 只有按下的是回车键，才会触发
    // console.log(e.key)
    if (e.key === 'Enter') {
        // 如果用户输入的不为空就显示和打印
        if (tx.value.trim()) {
            item.style.display = 'block'
            text.innerHTML = tx.value   // 用户输入的内容
        }
        // 等我们按下回车，结束，清空文本域
        tx.value = ''
        // 按下回车之后，就要把 字符统计 复原
        total.innerHTML = '0/200字'
    }
})

// .trim()方法，删除文字两侧的空格。如果全是空格，则删完是空字符串''
```

### 环境对象 this

指的是函数内部特殊的变量`this`，它代表着`当前函数运行时所处的环境`

函数调用方式不同，this指代的对象也不同——粗略判断：**谁调用，this就是谁**

直接调用函数的话，相当于`window.函数`，所以this指代window

```javascript
function fn() {
	//...
}
fn()		// 相当于window.fn()
```

```javascript
btn.addEventListener('click', function() {
	this.style.color = 'red'
})
```

### 回调函数

若将函数A作为参数传递给函数B时，称函数A为回调函数

```javascript
function fn() {
	// fn为setInterval的回调函数
}
setInterval(fn, 1000)
```

```javascript
box.addEventListener('click', function() {
	console.log('我也是回调函数')
})
```

使用匿名函数作为回调函数比较常见

**案例：Tab栏切换**

```javascript
const as = document.querySelectorAll('.tab-nav a')

for (let i = 0; i < as.length; i++) {
    // 要给 5个链接绑定鼠标经过事件
    as[i].addEventListener('mouseenter', function () {

        document.querySelector('.tab-nav .active').classList.remove('active')
        this.classList.add('active')

        document.querySelector('.tab-content .active').classList.remove('active')
        document.querySelector(`.tab-content .item:nth-child(${i + 1})`).classList.add('active')
    })
}
```

**表单全选反选案例**

```javascript
const checkAll = document.querySelector('#checkAll')
const cks = document.querySelectorAll('.ck')

checkAll.addEventListener('click', function() {
    for(let i = 0; i < cks.length; i++) {
    	cks[i].checked = this.checked	/* 全选按钮控制所有按钮的值，值一致 */
    }
})

for(let i = 0; i < cks.length; i++) {
    cks[i].addEventListener('click', function() {
    	checkAll.checked = document.querySelectorAll('.ck:checked').length === cks.length
        /* 全选了就是true，赋值给全选按钮勾上；否则为false，全选按钮不勾上 */
    })
}
```


## day3：DOM-事件进阶

### 事件流

**事件流**

- 定义：事件完整执行过程中的流动路径
- 两个阶段：`捕获阶段（父->子）`-> `冒泡阶段（子->父）`

**事件捕获（了解即可）**

- 从DOM的根元素开始去执行对应的事件（**从外到里**）

- 事件捕获需要写对应代码才能看到效果

  ```
  DOM.addEventListener(事件类型, 事件处理函数, 是否使用捕获机制)	
    // 第三个参数用true开启捕获阶段触发（少用），默认值为false
  ```

  ```javascript
  <div class="father">
      <div class="son"></div>
  </div>
  
  const fa = document.querySelector('.father')
  const son = document.querySelector('.son')
  
  document.addEventListener('click', function () {
    alert('我是爷爷')
  }, true)
  fa.addEventListener('click', function () {
    alert('我是爸爸')
  }, true)
  son.addEventListener('click', function () {
    alert('我是儿子')
  }, true)
  
  // 如果点击son盒子，捕获阶段：我是爷爷->我是爸爸->我是儿子；冒泡阶段：我是儿子->我是爸爸->我是爷爷
  ```

  若是L0(传统)事件监听，则只有冒泡阶段，没有捕获

**事件冒泡**

- 当一个元素触发事件后，会依次向上调用所有父级元素的同名/同类型事件（**由里到外**）
- 事件冒泡默认存在，即L2事件监听第三个参数默认是false
- 鼠标经过事件的区别
  - mouseover和mouseout有冒泡效果；mouseenter和mouseleave没有冒泡效果（推荐）

**阻止冒泡**

若想把事件就限制在当前元素内，就需要阻止冒泡

```
事件对象.stopPropagation()	// 阻止事件流动传播(捕获+冒泡)
```

```javascript
son.addEventListener('click', function (e) {
    alert('我是儿子')
    e.stopPropagation()
},)
```

**阻止元素默认行为**

```html
<form action="http://www.itcast.cn">
    <input type="submit" value="免费注册">
</form>

<a href="http://www.baidu.com">百度一下</a>
```

```javascript
const form = document.querySelector('form')
form.addEventListener('submit', function (e) {
    e.preventDefault()      // 阻止默认行为  提交
})

const a = document.querySelector('a')
a.addEventListener('click', function (e) {
    e.preventDefault()
})
```

**解绑事件**

1.对于`on事件方式`，直接赋值为null进行覆盖，就能实现事件的解绑

```javascript
btn.onclick = function() {
    alert('点击了')
}
// L0事件解绑
btn.onclick = null
```

2.对于`addEventListener方式`，用removeEventListener(事件类型，事件处理函数，[获取捕获或者冒泡阶段])

```javascript
function fn() {
    alert('点击了')
}
// 绑定事件
btn.addEventListener('click', fn)
// L2事件解绑
btn.removeEventListener('click', fn)
// 注意：匿名函数无法被解绑
```

总结：**L0事件和L2事件的区别**

- 传统on注册（L0）
  - 同一个对象，后面注册的事件会覆盖前面注册（同一个事件）
  - 直接使用null覆盖偶可以实现事件的解绑
  - 都是冒泡阶段执行的
- 事件监听注册（L2）
  - 语法：.addEventListener(事件类型, 事件处理函数, 是否使用捕获机制)	
  - 后面注册的事件不会覆盖前面注册的的事件（同一个事件）
  - 可以通过第三个参数去确定是在冒泡还是捕获阶段执行
  - 必须使用.removEventListener(事件类型, 事件处理函数, 是否使用捕获机制)解绑
  - 匿名函数无法被解绑

### 事件委托

- 优点：减少注册次数，可以提高程序性能

- 原理：**利用事件冒泡的特点**——给父元素注册事件，当触发子元素时，会冒泡到父元素身上，从而触发父元素的事件

- 实现：`事件对象.target.tagName` 可以获得真正触发事件的元素

```js
// 点击每个小li 当前li 文字变为红色
// 按照事件委托的方式  委托给父级，事件写到父级身上
// 1. 获得父元素
const ul = document.querySelector('ul')
ul.addEventListener('click', function (e) {
    // this.style.color = 'red'
    // console.dir(e.target) // 就是我们点击的那个对象
    // e.target.style.color = 'red'
    // 我的需求，我们只要点击li才会有效果
    if (e.target.tagName === 'LI') {
        e.target.style.color = 'red'
    }
})
// 点击一个li，会把跟这次点击事件相关的信息通过e传给函数中。
//其中e是该点击事件PointerEvent，e.target表示事件涉及的元素对象，即我们点击的那个li对象
// e.target.tagName即点击的对象的标签名(大写)，可以真正找到该元素
```

**案例：Tab栏切换改造**

```javascript
const ul = document.querySelector('.tab-nav ul')
const items = document.querySelectorAll('.tab-content .item')

ul.addEventListener('click', function (e) {
    if (e.target.tagName === 'A') {
        document.querySelector('.tab-nav .active').classList.remove('active')
        e.target.classList.add('active')

        const i = +e.target.dataset.id
        document.querySelector('.tab-content .active').classList.remove('active')
        // document.querySelector(`.tab-content .item:nth-child(${i + 1})`).classList.add('active')
        items[i].classList.add('active')
    }
})
```

### 其他事件

**1.页面加载事件**

1.load事件

页面加载外部资源（如图片、外联CSS和JavaScript等等），加载完毕时才去触发的事件——监听整个页面资源window

```javascript
// 等待页面所有资源加载完毕，就回去执行回调函数
window.addEventListener('load', function() {
    // 执行的操作
})
```

也可以针对某个资源绑定load事件

```js
img.addEventListener('load', function() {
    // 等待图片加载完毕，再去执行里面的代码
})
```

2.DOMContentLoaded事件

当初始的HTML文档（即所有的DOM元素）被完全加载和解析完成后，DOMContentLoaded事件被触发，而无需等待图片等外部资源完全加载——监听document

```javascript
document.addEventListener('DOMContentLoaded', function() {
    // ...
})
```

**2.页面滚动事件**

在滚动条滚动的时候持续触发的事件——监听整个页面滚动

```javascript
// 页面滚动事件
window.addEventListener('scroll', function() {
	// 执行的操作
})
```

也可以监听某个元素的内部滚动

**scrollLeft和scrollTop**——获取元素内容向左/向上滚动的距离；这两个值可读写

滚动的时候其实是最大的标签即html进行滚动

获取body：document.body

获取html：document.documentElement

```javascript
window.addEventListener('scroll', function() {
    const n = document.documentElement.scrollTop
    console.log(n)	// 数字型，不带单位；可读值也可赋值
})
```

回到顶部功能就是把scrollTop值赋值为0

滚动到指定坐标：`window.scrollTo(0, 1000)`

**3.页面尺寸事件**

resize会在浏览器页面尺寸改变的时候触发

```javascript
window.addEventListener('resize', function() {
	// 执行的代码
})
```

检测屏幕宽度

```javascript
window.addEventListener('resize', function() {
	let w = document.documentElement.clientWidth
	console.log(w)
})
```

**clientWidth和clientHeight**：获取元素可见部分宽高——除去border

### 元素尺寸与位置

**获取元素的大小（1）即整个宽高**——包含元素自身宽高、padding、border（若隐藏，则宽高为0）

——使用offsetWidth、offsetHeight——以自己的**父元素为参照物**——**只读属性**

**获取元素的位置**——参照物为**最近一级带有定位的祖先元素**

——使用offsetLeft、offsetTop——**只读属性**

案例：滚动条下滑出现固定顶部导航栏

```javascript
const sk = document.querySelector('.sk')
const header = document.querySelector('.header')
// 页面滚动事件
window.addEventListener('scroll', function () {
    // 当页面滚动到 秒杀模块的时候，就改变 头部的 top值
    // 页面被卷去的头部 >=  秒杀模块的位置 offsetTop
    const n = document.documentElement.scrollTop
    header.style.top = n >= sk.offsetTop ? 0 : '-80px'
})
```

**获取元素大小（2）**

element.getBoundingClientRect()——返回元素的大小及和目前相对于视口的位置

element.getBoundingClientRect().left、element.getBoundingClientRect().right


**综合案例：电梯导航★★★**

1.window滑动到大于某个高度时，电梯导航的poacity设为1，否则设为0。点击返回顶部就设置html的scrollTo(0,0)

2.点击电梯导航，设置active，并把对应list的offsetTop赋值给scrollTop

​	解决处理初次获取不到active报错的问题

- 不能直接获取这个类然后移除，这样会报错

- 先获取这个类，然后判断：如果有这个类，就移除；如果没有这个类，返回为null，就不执行移除，就不报错了

 ```javascript
 const old = document.querySelector('.xtx-elevator-list .active')
 // console.log(old)	// null
 // 判断 如果原来有active类的对象，就移除类，如果开始就没有对象，就不删除，所以不报错
 if (old) old.classList.remove('active')
 // 当前元素添加 active 
 e.target.classList.add('active')
 ```

 ```javascript
 const top = document.querySelector(`.xtx_goods_${e.target.dataset.name}`).offsetTop
 document.documentElement.scrollTop = top
 ```

3.当scrollTop在某两个list之间时，设置对应的list为active


## day4：DOM-节点操作

### 日期对象

用来表示时间的对象——可以得到当前系统时间

**实例化**

即通过new关键字创建一个对象

```javascript
// 获得系统当前时间
const date = new Date()
// 指定时间
const date1 = new Date('2023-8-8 08:30:00')

// 因为返回的日期格式无法直接使用，所以需要时间对象方法
```

**时间对象方法**

```javascript
// 获得日期对象
const date = new Date()
// 使用里面的方法
console.log(date.getFullYear())

/*
    - getFullYear()	    获得当前四位年份
    - getMonth()	    获得当前月份，0~11，所以要 + 1
    - getDate()		    获取当前的日
    - getDay()		    获取星期，0~6
    -getHours()、getMinutes()、getSeconds()	0~23/0~59,不会自动补零
*/
```

**案例：显示格式化的时间**

```javascript
const div = document.querySelector('div')
    
function getMyDate() {
    const date = new Date()
    let h = date.getHours()
    let m = date.getMinutes()
    let s = date.getSeconds()
    h = h < 10 ? '0' + h : h
    m = m < 10 ? '0' + m : m
    s = s < 10 ? '0' + s : s

    return `现在是：${date.getFullYear()}年${date.getMonth() + 1}月${date.getDate()}日 
              ${h}:${m}:${s}  星期${date.getDay()}`

}
div.innerHTML = getMyDate()	// 先写入一次，这样在开启定时器前的一秒div不会留空白
setInterval(function() {
    div.innerHTML = getMyDate()
}, 1000)
```

```javascript
// 快速获得时间的方式——对格式没有要求的时候

const div = document.querySelector('div')
// 得到日期对象
const date = new Date()
div.innerHTML = date.toLocaleString()  // 2022/4/1 09:41:21
setInterval(function () {
    const date = new Date()
    div.innerHTML = date.toLocaleString()  // 2022/4/1 09:41:21
}, 1000)

// div.innerHTML = date.toLocaleDateString()  // 2022/4/1
// div.innerHTML = date.toLocaleTimeString()  // 09:41:21
```

**时间戳**

使用场景：时间不太好进行加减运算，所以例如倒计时效果，需要借助时间戳完成

定义：是1970年第0秒至今的毫秒数，是一种特殊的计量时间的方式

算法

- 将来的时间戳 - 现在的时间戳 = 剩余时间毫秒数
- 剩余时间毫秒数 转换为 剩余时间的年月日时分秒 就是倒计时时间
- 比如：将来时间戳2000ms - 现在时间戳1000ms = 1000ms——转换为0时0分1秒

获取方式

```javascript
// 1.getTime()方法
const date = new Date()
console.log(date.getTime())
```

```javascript
// 2.+new Date()方法	- 这个最好
console.log(+new Date())
```

```javascript
// 3.Date.now()方法
console.log(Date.now())
```

前两种能返回任意时间的时间戳，可以用来做倒计时，但是第三种只能返回当前的时间戳。二三无需实例化

案例：倒计时功能

```javascript
const countdown = document.querySelector('.countdown')
countdown.style.backgroundColor = getRandomColor()
// 函数封装 getCountTime
function getCountTime() {
    // 1. 得到当前的时间戳
    const now = +new Date()
    // 2. 得到将来的时间戳
    const last = +new Date('2022-4-1 18:30:00')
    // console.log(now, last)
    // 3. 得到剩余的时间戳 count  记得转换为 秒数
    const count = (last - now) / 1000
    // console.log(count)
    // 4. 转换为时分秒
    let h = parseInt(count / 60 / 60 % 24)
    h = h < 10 ? '0' + h : h
    let m = parseInt(count / 60 % 60)
    m = m < 10 ? '0' + m : m
    let s = parseInt(count % 60)
    s = s < 10 ? '0' + s : s
    console.log(h, m, s)

    //  5. 把时分秒写到对应的盒子里面
    document.querySelector('#hour').innerHTML = h
    document.querySelector('#minutes').innerHTML = m
    document.querySelector('#scond').innerHTML = s
}
// 先调用一次
getCountTime()

// 开启定时器
setInterval(getCountTime, 1000)
```

### 节点操作

**DOM节点**

- DOM树里每一个内容都称之为节点
- 节点类型：元素节点、属性节点、文本节点、其他

**查找节点**

- 查找父节点

  ```javascript
  子元素.parentNode	// 返回最近一级的父节点，找不到返回null
  ```

- 查找子节点

  ```javascript
  父元素.childNodes	// 获得所有子节点，包括文本节点、注释节点等等	不常用
  父元素.children	// 仅获得所有元素节点亲儿子，返回的是一个伪数组
  ```

- 查找兄弟节点

  ```javascript
  nextElementSibling	// 下一个兄弟节点
  previousElementSibling	// 上一个兄弟节点
  ```

**增加节点**

- 1.创建节点

  ```javascript
  document.createElement('标签名')
  ```

- 2.追加节点

  - 插入到父元素的最后一个子元素

    ```javascript
    父元素.appendChild(要插入的元素)
    ```

  - 插入到父元素的某个子元素前面

    ```javascript
    父元素.insertBefore(要插入的元素，在哪个元素前面)
    
    例：ul.insertBefore(li, ul.children[0])
    ```

- 克隆节点

  ```javascript
  元素.cloneNode(布尔值)
  
  /*	克隆一个已有的元素结点，括号内传入布尔值	- 深克隆
  	若为true，则克隆时包括后代节点一并克隆	  - 潜克隆
  	默认为false，不克隆后代节点
  */
  
  例：ul.appendChild(ul.children[0].cloneNode(true))
  ```

**删除节点**

在JS原生DOM操作中，要删除元素必须通过父元素删除

```javascript
父元素.removeChild(要删除的元素)
```

### M端事件

了解移动端常见的事件

| 触屏touch事件 | 说明                          |
| ------------- | ----------------------------- |
| touchstart    | 手指触摸到一个DOM元素时触发   |
| touchmove     | 手指在一个DOM元素上滑动时触发 |
| touchend      | 手指从一个DOM元素上移开时触发 |

### JS插件

M端的JS插件Swiper：https://www.swiper.com.cn/ ,Swiper常用于移动端网站的内容触摸滑动

下载完毕后找到package文件夹，把css、js粘到项目中就好

在线演示中找到想要的效果，打开源代码，慢慢cv

## day5：BOM-操作浏览器

### 一、Window对象

学习目标：学习window对象的常见属性，知道各个BOM对象的功能含义

### BOM-浏览器对象模型

window = navigator + location + document + history + screen

- window对象是一个全局对象，可以说是JavaScript中的顶级对象
- 基本BOM的属性和方法都是window的；像document、alert()、console.log()这些都是window的属性
- 所有通过var定义在全局作用域中的变量、函数都会变成window对象的属性和方法
- window对象下的属性和方法调用的时候可以省略window

### 定时器 - 延时函数

JavaScript中内置了一个延时执行代码的函数（仅执行一次），setTimeout

```javascript
setTimeout(回调函数，等待的毫秒数)
```

清除延时函数：

```javascript
let timer = setTimeout(回调函数，等待的毫秒数)
clearTimeout(timer)
```

注意：间歇函数是每隔一段时间执行一次，而延时函数只执行一次就不再执行

### JS执行机制

JavaScript最大的一个特点是单线程，即同一时间只能做一件事

即所有任务都需要排队，前一个任务执行完才会执行下一个任务

导致问题：如果JS执行的时间过长，会造成页面的渲染不连贯，导致页面渲染加载阻塞的感觉

为了解决这个问题，利用多核CPU的计算能力，H5提出Web Worker标准，允许JavaScript脚本创建多个线程

于是JavaScript出现了`同步`和`异步`

**同步**：前一任务执行完再执行下一任务，程序的执行顺序和任务的排列顺序是一致的、同步的

**异步**：在做一件事要花费很多时间，在这同时再去做其他事情

同步任务：都在主线程上执行，形成一个执行栈

异步任务：通过回调函数实现

- 普通事件，如click，resize等
- 资源加载，如load，errir等
- 定时器，包括setInterval、setTimeout等

异步任务相关添加到任务队列(消息队列)中

1. 先执行执行栈中的同步任务；异步任务放入任务队列中
2. 当执行栈中所有同步任务执行完毕，系统就按次序读取任务队列中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行

<img src="前端学习笔记★.assets/image-20230827083624308.png" alt="image-20230827083624308" style="zoom: 67%;" />

异步API部分是交给浏览器执行，浏览器可以多线程

由于主线程不断的重复获得任务、执行任务，一直循环——这种机制被称为`事件循环(event loop)`

### location对象

location的数据类型是对象，它拆分并保存了URL地址的各个组成部分

- href

```
location.href       // 得到当前文件URL地址
location.href = 'http://www.itcast.cn'  // 可以通过js方式跳转到目标地址
```

```javascript
// 案例：点击跳转或者倒计时结束自动跳转
<a href="http://www.itcast.cn">支付成功，<span>5</span>秒钟后跳转到首页</a>
<script>
    a = document.querySelector('a')
    let num = 5
    let timerId = setInterval(function() {
        num--
        a.innerHTML = `支付成功，<span>${num}</span>秒钟后跳转到首页`
        if(num === 0) {
            clearInterval(timerId)
            location.href = 'http://www.itcast.cn'
        }
    }, 1000)
</script>
```

- search 属性获取地址中携带的参数，符号？及后面部分——例如表单get提交时？后面的内容

- hash 属性获取地址中的哈希值，符号#后面部分——例如‘#/download’。经常用于不刷新页面就可以显示不同页面
- reload() 方法用来刷新当前页面，传入参数true时表示强制刷新

### navigator对象

navigator的数据类型是对象，该对象下记录了浏览器自身的相关信息

- 通过userAgent检测浏览器的版本及平台

```javascript
// 检测 userAgent（浏览器信息）
!(function () {
    const userAgent = navigator.userAgent
    // 验证是否为Android或iPhone
    const android = userAgent.match(/(Android);?[\s\/]+([\d.]+)?/)
    const iphone = userAgent.match(/(iPhone\sOS)\s([\d_]+)/)
    // 如果是Android或iPhone，则跳转至移动站点
    if (android || iphone) {
        location.href = 'http://m.itcast.cn'
    }
})();
// !function () { }()   也是立即执行函数，加！可以把前面看成一个整体。不加！会报错
```

### history对象

history的数据类型是对象，主要管理历史记录，该对象与浏览器地址栏的操作相对应，如前进、后退、历史记录等

该对象一般在实际开发中**比较少用**

- back()方法：后退功能
- forward()方法：前进功能
- go(参数) 方法：参数为正数就前进几个页面，为负数就后退几个页面

### 二、本地存储

本地存储介绍

- 数据存储在用户浏览器中
- 设置、读取方便，甚至页面刷新不丢失数据
- 容量较大，sessionStorage和localStorage约5M左右

本地存储分类

### **1.localStorage**

作用：可以将数据永久存储在本地（用户的电脑），除非手动删除，否则关闭页面也会存在

特性：可以同一浏览器多页面、多窗口共享(跨域不行)；以键值对的形式存储使用

语法：

```javascript
localStorage.setItem('key', 'value')        // 存储数据、已有key时就是改数据
localStorage.getItem('key')                 // 获取数据
localStorage.removeItem('key')              // 删除数据

// 本地存储只能存储字符串数据类型，返回值也是字符串
```

### 2.sessionStorage

特性：

- 生命周期为关闭浏览器窗口
- 在同一个窗口/页面下数据可以共享
- 以键值对的形式存储使用
- 用法跟localStorage基本相同

### 存储复杂数据类型

问题：本地只能存储字符串，无法存储复杂数据类型

```javascript
const obj = {
    uname: 'pink老师',
    age: 18,
    gender: '女'
}
localStorage.setItem('obj', obj)
console.log(localStorage.getItem('obj'))
```

| key  | value           |
| ---- | --------------- |
| obj  | [object Object] |

解决：需要将**复杂数据类型**`转换成`**JSON字符串**，再存储到本地

语法：

```javascript
JSON.stringify(复杂数据类型)
JSON.parse(JSON字符串)
```

```javascript
const obj = {
    uname: 'pink老师',
    age: 18,
    gender: '女'
}
localStorage.setItem('obj', JSON.stringify(obj))
console.log(localStorage.getItem('obj'))    // {"uname": "pink老师", "age": 18, "gender": "女"}
                                            // JSON对象，属性和值统一都有双引号""
console.log(JSON.parse(localStorage.getItem('obj')))        // JSON.parse()方法 将JSON字符串转换成JS对象
```

### 综合案例

day4+5 ——学生信息表实时渲染案例（待完成）

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>学生信息表渲染案例</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        a {
            text-decoration: none;
            color: #721c24;
        }

        h1 {
            text-align: center;
            color: #333;
            margin: 20px 0;
        }

        table {
            margin: 20px auto;
            width: 800px;
            border-collapse: collapse;
            color: #004085;
        }

        th {
            padding: 10px;
            background: #cfe5ff;

            font-size: 20px;
            font-weight: 400;
        }

        td,
        th {
            border: 1px solid #b8daff;
        }

        td {
            padding: 10px;
            color: #666;
            text-align: center;
            font-size: 16px;
        }

        tbody tr {
            background: #fff;
        }

        tbody tr:hover {
            background: #e1ecf8;
        }

        .info {
            width: 900px;
            margin: 50px auto;
            text-align: center;
        }

        .info input,
        .info select {
            width: 80px;
            height: 27px;
            outline: none;
            border-radius: 5px;
            border: 1px solid #b8daff;
            padding-left: 5px;
            box-sizing: border-box;
            margin-right: 15px;
        }

        .info button {
            width: 60px;
            height: 27px;
            background-color: #004085;
            outline: none;
            border: 0;
            color: #fff;
            cursor: pointer;
            border-radius: 5px;
        }

        .info .age {
            width: 50px;
        }
    </style>
</head>

<body>
    <h1>新增学员</h1>
    <form class="info" autocomplete="off">
        姓名：<input type="text" class="uname" name="uname" />
        年龄：<input type="text" class="age" name="age" />
        性别：<select class="gender" name="gender">
            <option value="男">男</option>
            <option value="女">女</option>
        </select>
        薪资：<input type="text" class="salary" name="salary" />
        就业城市：<select class="city" name="city">
            <option value="北京">北京</option>
            <option value="上海">上海</option>
            <option value="广州">广州</option>
            <option value="深圳">深圳</option>
        </select>
        <button class="add">录入</button>
    </form>

    <h1>就业榜</h1>
    <table>
        <thead>
            <tr>
                <th>学号</th>
                <th>姓名</th>
                <th>年龄</th>
                <th>性别</th>
                <th>薪资</th>
                <th>就业城市</th>
                <th>操作</th>
            </tr>
        </thead>
        <tbody>
            <!-- <tr>
                <td>1001</td>
                <td>欧阳霸天</td>
                <td>19</td>
                <td>男</td>
                <td>15000</td>
                <td>上海</td>
                <td>
                    <a href="javascript:">删除</a>
                </td>
            </tr> -->
        </tbody>
    </table>

    <script>
        // 参考数据
        // const initData = [
        //     {
        //         stuId: 1001,
        //         uname: '欧阳霸天',
        //         age: 19,
        //         gender: '男',
        //         salary: '20000',
        //         city: '上海',
        //     }
        // ]

        // 读取本地的 student-data 的数据
        const data = localStorage.getItem('student-data')
        const arr = data ? JSON.parse(data) : []

        const tbody = document.querySelector('tbody')

        // ——渲染模板函数
        function render() {
            const trArr = arr.map(function (item, i) {
                return `
                <tr>
                    <td>${item.stuId}</td>
                    <td>${item.uname}</td>
                    <td>${item.age}</td>
                    <td>${item.gender}</td>
                    <td>${item.salary}</td>
                    <td>${item.city}</td>
                    <td>
                        <a href="javascript:" data-id=${i}>删除</a>
                    </td>
                </tr> 
                `
            })

            tbody.innerHTML = trArr.join('')
        }
        render()

        // ——录入模块
        const info = document.querySelector('.info')
        const items = info.querySelectorAll('[name]')

        info.addEventListener('submit', function (e) {
            // 阻止提交表单的默认行为
            e.preventDefault()

            const obj = {}
            obj.stuId = arr.length ? arr[arr.length - 1].stuId + 1 : 1

            // 非空判断
            for (let i = 0; i < items.length; i++) {
                const item = items[i]
                if (items[i].value === '') {
                    return alert('输入不能为空')
                }
                // item.name为当前处理的表单元素的name属性值(名)，item.value为当前正在处理的表单元素的值；这里实际是在添加键值对
                obj[item.name] = item.value
            }
            arr.push(obj)

            // arr存储到本地存储
            localStorage.setItem('student-data', JSON.stringify(arr))
            // 渲染页面
            render()
            // 重置表单
            this.reset()
        })

        // ——删除模块
        tbody.addEventListener('click', function (e) {
            arr.splice(e.target.dataset.id, 1)
            localStorage.setItem('student-data', JSON.stringify(arr))
            render()
        })

    </script>
</body>

</html>
```



## day6：正则表达式

### 基本介绍

正则表达式是用于匹配字符串中字符组合的模式

作用：

- 验证表单（匹配）
- 过滤敏感词（替换）
- 字符串中提取想要的部分（提取）

语法：

```javascript
const str = '我们在学习前端，希望学习前端能高薪毕业'
// 正则表达式使用：
// 1. 定义规则
const reg = /前端/
// 2. 是否匹配
console.log(reg.test(str))  // 返回true   test()方法——判断是否有符合规则的字符串，找到返回true，否则false
// 3. exec()
console.log(reg.exec(str))  // 返回数组     exec()方法——用于检索(查找)符合规则的字符串，找到返回数组，否则null
```

### 元字符

普通字符：仅描述本身的字符，例如所有字母和数字

元字符：具有特殊含义的字符，可以极大提高灵活性，有强大的匹配功能，例如**[a-z]**表示abcd...xyz

分类：

- **边界符**——提示字符所处的位置，即必须用什么开头，用什么结尾

```javascript
^           表示匹配行首的文本（即以谁开始）
$           表示匹配行尾的文本（即以谁结束）
————————————————————————————————————————————————————
console.log(/^二哈/.test('二哈笑哈哈'))        // true
console.log(/^二哈/.test('傻子二哈'))         // false

console.log(/二哈$/.test('这是一只傻二哈'))      // true
console.log(/二哈$/.test('这只二哈很傻'))       // false

console.log(/^二哈$/.test('二哈笑哈哈'))       // false
console.log(/^二哈$/.text('二哈'))              // true --> ^和$一起必须是精确匹配
```

- **量词**——设定某个模式出现的次数，即重复次数

```javascript
*       重复0、多次
+       重复1、多次
?       重复0、1次
{n}     重复n次
{n,}    重复n、更多次
{n,m}   重复n~m次
——————————————————————————————————————————————————
console.log(/^哈{4,}$/.test('哈哈哈'))      // false
console.log(/^哈{4,}$/.test('哈哈哈哈'))     // true
console.log(/^哈{4,}$/.test('哈哈哈哈哈'))    // true
```

- **字符类**——比如`\d`表示0~9

```javascript
[]      匹配字符集合，只要匹配到内部任何一个字符都返回true，只匹配单个字符
[^a-z]  表示匹配除了小写字母以外的字符
.       匹配除换行符以外的任何单个字符

console.log(/^[abc]{2}$/.test('ab'))    // true
console.log(/[abc]/.test('die'))    // flase
console.log(/^[a-zA-Z0-9]$/.test('P'))  // true

腾讯QQ号格式(从1000开始)：^[1-9][0-9]{4,}$
```

```javascript
// 案例：验证用户名格式
const reg = /^[a-zA-Z0-9-_]{6,16}$/
const input = document.querySelector('input')
const span = input.nextElementSibling

input.addEventListener('blur', function () {
    // console.log(reg.test(this.value))
    if (reg.test(this.value)) {
        span.innerHTML = '输入正确'
        span.className = 'right'
    } else {
        span.innerHTML = '请输入6~16位的英文数字下划线'
        span.className = 'error'
    }
})
```

```javascript
预定义类——某些常见模式的简写方法

\d      [0-9]
\D      [^0-9]
\w      [a-zA-Z0-9_]
\W      [^a-zA-Z0-9_]
\s      [\t\r\n\v\f]
\S      [^\t\r\n\v\f]

日期格式：^\d{4}-\d{1,2}-\d{1,2}
```

### 修饰符

修饰符约束正则执行的某些细节行为，如是否区分大小写、是否支持多行匹配等等

```javascript
格式：/表达式/修饰符

i           即ignore，正则匹配时字母不区分大小写
g           即global，匹配所有满足正则表达式的结果
replace     找到想要的文本进行替换


const str = 'java是一门编程语言， 学完JAVA工资很高'
// const re = str.replace(/java|JAVA/g, '前端')
const re = str.replace(/java/ig, '前端')
console.log(re)         // 输出：前端是一门编程语言， 学完前端工资很高


btn.addEventListener('click', function() {
    div.innerHTML = tx.value.replace(/激情|基情/g, '**')
})
```

**案例：小兔鲜注册页的输入验证、登录页面、主界面导航栏用户名**

## day7：实战-小兔鲜个人实战

### 放大镜效果

**业务分析：**

①：鼠标经过对应小盒子，左侧中等盒子显示对应中等图片

②： 鼠标经过中盒子，右侧会显示放大镜效果的大盒子

③： 黑色遮罩盒子跟着鼠标来移动

④： 鼠标在中等盒子上移动，大盒子的图片跟着显示对应位置



**思路分析：**

①：鼠标经过小盒子，左侧中等盒子显示对应中等图片

1. 获取对应的元素

2. 采取事件委托的形式，监听鼠标经过小盒子里面的图片， 注意此时需要使用 `mouseover` 事件，因为需要事件冒泡触发small 

3. 让鼠标经过小图片的爸爸li盒子，添加类，其余的li移除类（注意先移除，后添加）

4. 鼠标经过小图片，可以拿到小图片的src， 可以做两件事

     \- 让中等盒子的图片换成这个 这个小图片的src
    
     \- 让大盒子的背景图片，也换成这个小图片的 src （稍后做）



②： 鼠标经过中等盒子，右侧大盒子显示

1. 用到鼠标经过和离开，鼠标经过中盒子，大盒子 利用 display 来显示和隐藏

2. 鼠标离开不会立马消失，而是有200ms的延时，用户体验更好，所以尽量使用定时器做个延时 setTimeout

3. 显示和隐藏也尽量定义一个函数，因为鼠标经过离开中等盒子，会显示隐藏；同时，鼠标经过大盒子，也会显示和隐藏

4. 给大盒子里面的背景图片一个默认的第一张图片



③： 黑色遮罩盒子跟着鼠标来移动

1. 先做鼠标经过 中等盒子，显示隐藏 黑色遮罩 的盒子

2. 让黑色遮罩跟着鼠标来走, 需要用到鼠标移动事件  mousemove  

3. 让黑色盒子的移动的核心思想：不断把鼠标在中等盒子内的坐标给黑色遮罩层 let  top 值，这样遮罩层就可以跟着移动了

     \- 需求
    
      \- 我们要的是 鼠标在 中等盒子内的坐标， 没有办法直接得到

​      \- 得到1：  鼠标在页面中的坐标

​      \- 得到2：  中等盒子在页面中的坐标



  \- 算法

   \- 得到鼠标在页面中的坐标   利用事件对象的  pageX  

   \- 得到middle中等盒子在页面中的坐标  middle.getBoundingClientRect()

   \- 鼠标在middle 盒子里面的坐标  =  鼠标在页面中的坐标  -  middle 中等盒子的坐标

   \- 黑色遮罩层不断得到    鼠标在middle 盒子中的坐标 就可以移动起来了



   *>注意 y坐标特殊，需要减去 页面被卷去的头部* 

   *>*

   *>为什么不用 box.offsetLet 和 box.offsetTop  因为这俩属性跟带有定位的父级有关系，很容被父级影响，而getBoundingClientRect() 不受定位的父元素的影响*



  \- 限定遮罩的盒子只能在middle 内部移动，需要添加判断



   \- 限定水平方向 大于等于0 并且小于等于 400

   \- 限定垂直方向 大于等于0 并且小于等于 400



  \- 遮罩盒子移动的坐标： 



   \- 声明一个 mx 作为移动的距离

   \- 水平坐标 x 如果 小于等于100 ，则移动的距离 mx 就是  0  不应该移动

   \- 水平坐标 如果 大于等于100 并且小于300，移动的距离就是  mx - 100 （100是遮罩盒子自身宽度的一半）

   \- 水平坐标 如果 大于等于300，移动的距离就是  mx  就是200  不应该在移动了

   \- 其实我们发现水平移动， 就在 100 ~ 200 之间移动的

   \- 垂直同理

```javascript
let mx = 0, my = 0;

if (x <= 100) mx = 0
if (x > 100 && x < 300) mx = x - 100
if (x >= 300) mx = 200

if (y <= 100) my = 0
if (y > 100 && y < 300) my = y - 100
if (y >= 300) my = 200
```



  \- 大盒子图片移动的计算方法：

   \- 中等盒子是 400px  大盒子 是 800px 的图片

   \- 中等盒子移动1px， 大盒子就应该移动2px， 只不过是负值

```javascript
large.style.backgroundPositionX = - 2 * mx + 'px'

large.style.backgroundPositionY = - 2 * my + 'px'
```
