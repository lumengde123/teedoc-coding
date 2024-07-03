---
title: CSS3 学习笔记
keywords: CSS3
desc: CSS3学习笔记
date: 2024-06-16
class: heading_no_counter
tags: 笔记, CSS3
---

# CSS3的现状

- 新增的CSS3特性有兼容性问题，ie9+才支持
- 移动端支持优于PC端
- 不断改进中
- 应用相对广泛
- 现阶段主要学习：新增选择器和盒子模型以及其他特性

# 新增选择器

CSS3给我们新增了三个强大的选择器，可以更快更便捷的选择目标元素

- 属性选择器
- 结构伪类选择器
- 伪元素选择器

## 属性选择器

```css
<style>
    /* 必须是input 但是同时具有 value这个属性 选择这个元素  [] */
    input[value] {
        color:pink;
    }
    /* 只选择 type =text 文本框的input 选取出来 */
    input[type=text] {
        color: pink;
    }
    /* 选择首先是div 然后 具有class属性 并且属性值 必须是 icon开头的这些元素 */
    div[class^=icon] {
        color: red;
   }
    section[class$=data] {
        color: blue;
    }
    div.icon1 {
        color: skyblue;
    }
    /* 类选择器和属性选择器 伪类选择器 权重都是 10 */
</style>
```



| 选择符        | 简介                                              |
| ------------- | ------------------------------------------------- |
| E[att]        | 选择具有 att 属性的 E 元素                        |
| E[att="val"]  | 选择具有 att 属性 且属性值为 val 的 E 元素        |
| E[att^="val"] | 匹配具有 att 属性 且 值以 val 开头的 E 元素       |
| E[att$="val"] | 匹配具有 att 属性的元素 且 值以 val 结尾的 E 元素 |
| E[att*="val"] | 匹配具有 att 属性 且 值中含有 val 的 E 元素       |

- **类选择器**、**属性选择器**、**伪类选择器**、权重都是 **10**

| 选择器                           | 选择器权重 |
| -------------------------------- | ---------- |
| 继承 / *                         | 0, 0, 0, 0 |
| 标签选择器                       | 0, 0, 0, 1 |
| 类选择器，属性选择器，伪类选择器 | 0, 0, 1, 0 |
| ID选择器                         | 0, 1, 0, 0 |
| 行内样式 style=""                | 1, 0, 0, 0 |
| !important 重要的                | ∞          |

**注意！** **继承的权重是0**。如果给自己写了样式，那么将会覆盖继承的样式

## 结构伪类选择器

主要根据文档结构来选择元素，常用于根据父级选择里面的子元素

| 选择符           | 简介                         |
| ---------------- | ---------------------------- |
| E:first-child    | 匹配父元素中的第一个子元素 E |
| E:last-child     | 匹配父元素中最后一个 E 元素  |
| E:nth-child(n)   | 匹配父元素中的第 n 个子元素E |
| E:first-of-type  | 指定类型 E 的第一个          |
| E:last-of-type   | 指定类型 E 的最后一个        |
| E:nth-of-type(n) | 指定类型 E 的第 n 个         |






























