---
title: 前端三剑客：HTML、CSS、JavaScript 学习笔记
keywords: html, css, javascript, 笔记, 教程
desc: 前端学习笔记
date: 2024-06-15
class: heading_no_counter
---

前端是学习B站黑马程序员的课程，这里是整理的课程对应的笔记。

## 学习进度

- HTML+CSS，up：黑马pink讲前端，视频课程：BV14J4114768，项目文件：https://gitee.com/xiaoqiang001/html_css_material ，百度网盘已保存

- JavaScript，up：黑马程序员，视频课程：BV1Y84y1L7Nn，项目文件：百度网盘已保存

学习进度：css P265,  JS Day3-35

文档更新进度：
- [x] HTML 内容校对
- [x] HTML 内容补充
- [ ] CSS 内容校对——元素显示模式
- [ ] CSS 内容补充
- [ ] ECMAScript 内容校对
- [ ] ECMAScript 内容补充
- [ ] web-apis-dom 内容校对
- [ ] web-apis-dom 内容补充
- [ ] js进阶 内容校对
- [ ] js进阶 内容补充

## 一些小技巧、小工具

## 辅助工具

xpath、FastStone Capture、

Fireworks，是Adobe公司出品的一款网页制作软件，是一款专为网络图形设计的图形编辑软件

Just.Color.Picker 5.5：一个小巧简洁的取色工具

snipaste：可以让截图贴回到屏幕上		**QQ也能实现：ctrl+alt+a进入截图模式，按ctrl+c取色，按esc取消置顶**

- F1截图，同时可以测量大小，设置箭头，书写文字等
- F3在桌面置顶显示
- 点击图片，alt可以取色（按下shift可以切换取色模式）
- 按下esc取消图片显示

像素大厨（切图工具）

Diagram Designer（画流程图）

Zoomlt64.exe：可以放大缩小屏幕，在屏幕上画图



## Emmet语法

Emmet语法的前身是Zen coding，它使用缩写，来提高html/css的编写速度，vscode内部已经集成该语法

```markdown
# Emmet快速生成HTML标签
按 tab 键即可快速生成标签
div + tab	-->	生成<div></div>
div*3		-->	快速生成三个div
tr>td*5  	-->	快速生成tr里面包着5个td（父子级）
div+p		-->	快速生成div和p（兄弟级）
.demo或#two		-->	快速生成class="demo"或id="two"的标签
.demo$*5	-->	快速生成类名为demo1~demo5的五个标签
div{这是第$个句子}*5	-->生成五个文本内容为“这是第(1~5)个句子的div”

div.demo$*5+ul#two>li*2	-->	快速生成class为demo1~demo5的五个div标签和id="two"包含两列的无序列表
```

```markdown
# Emmet快速生成CSS样式

tac		--> text-align: center;
ti2em	--> text-indent: 2em;
w100	--> width: 100px;
lh26px	--> line-height: 26px;
```



## chrome调试工具

chrome浏览器按F12可召唤调试工具

1. Ctrl+滚轮 可以放大开发者工具代码大小
2. 左边是HTML元素结构，右边是CSS样式
3. 右边CSS样式可以改动数值（左右箭头或者直接输入）和查看颜色
4. Ctrl+0 复原浏览器大小
5. 如果点击元素，发现**右侧没有样式引入**，极有可能是**类名或者样式引入错误**
6. 如果有样式，但是**样式前面有黄色感叹号提示**，则是**样式属性书写错误**

## vscode自动格式化代码

设置 --> 设置 --> 文本编辑器 --> 格式化 --> Format On Save

快捷键：

```markdown
alt + ↓ 	-- 可以将当前行拷贝到下一行	
ctrl + g	-- 输入数字，跳转到指定行

shift + alt + 左键下拉光标到下几行，然后拖动光标  
  -- 可以多行同步选中，复制了多行也可以多行直接粘贴进去
```
