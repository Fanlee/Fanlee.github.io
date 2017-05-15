# css之position属性
---
title: css之position属性
date: 2017-05-12
categories: 学习笔记
tags: CSS
keywords: CSS
description: position
comments: true
---

## 简介
> position CSS属性选择用于定位元素的替代规则，被设计为对脚本动画效果有用。

## 语法
```
/* 关键字  值 */
position: static;
position: relative;
position: absolute;
position: fixed;
position: sticky; //实验性API

/* 全局值 */
position: inherit;
position: initial;
position: unset;
```
### static 元素默认定位属性
默认值。没有定位，元素出现在正常的文档流中，忽略top, right, bottom, left, z-index声明。

### relative 相对定位
相对自己文档流中的原始位置进行定位，**不会脱离文档流**。
![](http://optwq0urg.bkt.clouddn.com/blog/20170512/165119169.png?imageslim)
上图给test4加上了position:relative效果，代码如下
```
position: relative; top:10px; left:-20px
```
可以看出test4并没有对周围的元素造成影响， 它还是存在于正常的文档流中。position:relative对 table-*-group, table-row, table-column, table-cell, table-caption 元素没有效果。

### absolute 绝对定位
1. 相对于static定位以外的第一个父元素进行定位， **脱离文档流**
![mark](http://optwq0urg.bkt.clouddn.com/blog/20170512/170435114.png?imageslim)
上图给test4加上了position:absolute效果
明显可以看到test4从正常的文档流中脱离了出来， test5填补了test4原本的位置。 

2. 绝对定位元素可以设置外边距，且不会与其他边距合并
CSS代码
```
body{
  margin: 0;
  padding: 0;
}
.box {
  position: relative;
  width: 500px;
  height: 200px;
  background: lightseagreen;
  margin: 30px
}
.box1 {
  position: absolute;
  top: 0;
  left: 0;
  margin: 20px;
  width: 100px;
  height: 50px;
  background: red
}eight: 50px;
background: red
}
```
HTML代码
```
<div class="box">
  <div class="box1">absolute定位元素</div>
</div>
```
效果图
![mark](http://optwq0urg.bkt.clouddn.com/blog/20170512/171943995.png?imageslim)

### sticky
position:sticky是一个新的CSS3属性，它的表现类似position:relative和position:fixed的合体，在目标区域在屏幕中可见时，它的行为就像position:relative; 而当页面滚动超出目标区域时，它的表现就像position:fixed，它会固定在目标位置
CSS代码
```
.container {
  background: #eee;
  width: 600px;
  height: 1000px;
  margin: 0 auto;
}

.sticky-box {
  position: -webkit-sticky;
  position: sticky;
  height: 60px;
  margin-bottom: 30px;
  background: #ff7300;
  top: 0px;
}

div {
  font-size: 30px;
  text-align: center;
  color: #fff;
  line-height: 60px;
}
```
HTML代码
```
<div class="container">
    <div class="sticky-box">内容1</div>
    <div class="sticky-box">内容2</div>
    <div class="sticky-box">内容3</div>
    <div class="sticky-box">内容4</div>
</div>
```
效果图
![mark](http://optwq0urg.bkt.clouddn.com/blog/20170515/100352640.png?imageslim)
因为设定的阈值是 top:0 ，这个值表示当元素距离页面视口（Viewport，也就是fixed定位的参照）顶部距离大于 0px 时，元素以 relative 定位表现，而当元素距离页面视口小于 0px 时，元素表现为 fixed 定位，也就会固定在顶部。
浏览器支持情况
![mark](http://optwq0urg.bkt.clouddn.com/blog/20170515/100532644.png?imageslim)
IOS 家族（SAFARI && IOS SAFARI）和 Firefox 很早开始就支持 position:sticky 了。而 Chrome53~55 则需要启用实验性网络平台功能才行。其中 webkit 内核的要添加上私有前缀 -webkit-。




