---
title: Vue.js Course for Beginners
keywords: vue
desc: vue学习笔记
date: 2024-06-18
class: heading_no_counter
tags: 笔记, vue
---

视频链接：[Vue.js Course for Beginners [2021 Tutorial] - Youtube](https://www.youtube.com/watch?v=FXpIoQ_rT_c)

## Vue 3 Setup

```html
<body>
	<div id="app">
		{{ greeting }}
	</div>
	<script src="https://unpkg.com/vue"></script>
	<script>
		let app = Vue.createApp({
			data: function() {
				return {
					greeting: 'Hello Vue 3!'
				}
			}
		})
		app.mount('#app')
	</script>
</body>
```

Vue.createApp() 函数创建了一个新的 Vue 应用实例并返回给变量`app`，

Vue.createApp()接受一个**选项对象**作为参数，这个选项对象定义了应用的配置。

在这个选项对象中，data 函数用来定义应用的初始数据，然后会返回一个定义了应用的初始数据的对象。

这是data()函数的固定写法，里面除了`return`不能包含其他逻辑或语句
```
data() {
	return {
		// 初始化的属性和方法
	}
}
```

- data(){}是官方推荐的ES6写法，更简洁
- data: function() {} 是传统写法

app.mount()方法将 Vue 应用或组件实例挂载到一个 DOM 元素上。挂载过程将 Vue 的虚拟 DOM 和实际的 DOM 关联起来，从而使得 Vue 可以管理和更新 DOM 元素。

这里通过mount()方法将app应用组件挂载到了id为app的DOM元素(div)上

通过模板语法`{{ }}`，将`greeting`属性的内容替换到对应的`div`块中去

## Vue.js Directives(指令)

指令由 `v-`` 作为前缀，表明它们是一些由 Vue 提供的特殊 attribute，它们将为渲染的 DOM 应用特殊的响应式行为。

```html
<div v-if="isVisible" class="box"></div>
<div v-else-if="isVisible2" class="box two"></div>
<div v-else class="box three"></div>
```

- 用v-if，不满足条件时代码会直接删除
- 用v-show，值是boolean类型，不满足条件时代码会display:none而非直接删除，适用于频繁toggle的情况，
