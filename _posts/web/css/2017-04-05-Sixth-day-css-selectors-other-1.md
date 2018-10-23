---
title: CSS3 选择器-其他1
key: 20170405-01
tags: css3
comment: true
---

## CSS3 first-of-type选择器

“:first-of-type”选择器类似于“:first-child”选择器，不同之处就是指定了元素的类型,其主要用来定位一个父元素下的某个类型的第一个子元素。

示例演示：

通过“:first-of-type”选择器，定位div容器中的第一个p元素（p不一定是容器中的第一个子元素），并设置其背景色为橙色。

HTML代码：
```html
<div class="wrapper">
  <div>我是一个块元素，我是.wrapper的第一个子元素</div>
  <p>我是一个段落元素，我是不是.wrapper的第一个子元素，但是他的第一个段落元素</p>
  <p>我是一个段落元素</p>
  <div>我是一个块元素</div>
</div>
```
CSS代码：
```css
.wrapper {
  width: 500px;
  margin: 20px auto;
  padding: 10px;
  border: 1px solid #ccc;
  color: #fff;
}
.wrapper > div {
  background: green;
}
.wrapper > p {
  background: blue;
}
/*我要改变第一个段落的背景为橙色*/
.wrapper > p:first-of-type {
  background: orange;
}
```
演示结果：

![][1]


## CSS3 nth-of-type(n)选择器

“:nth-of-type(n)”选择器和“:nth-child(n)”选择器非常类似，不同的是它只计算父元素中指定的某种类型的子元素。当某个元素中的子元素不单单是同一种类型的子元素时，使用“:nth-of-type(n)”选择器来定位于父元素中某种类型的子元素是非常方便和有用的。在“:nth-of-type(n)”选择器中的“n”和“:nth-child(n)”选择器中的“n”参数也一样，可以是具体的整数，也可以是表达式，还可以是关键词。

示例演示

通过“:nth-of-type(2n)”选择器，将容器“div.wrapper”中偶数段数的背景设置为橙色。

HTML代码：
```html
<div class="wrapper">
  <div>我是一个Div元素</div>
  <p>我是一个段落元素</p>
  <div>我是一个Div元素</div>
  <p>我是一个段落</p>
  <div>我是一个Div元素</div>
  <p>我是一个段落</p>
  <div>我是一个Div元素</div>
  <p>我是一个段落</p>
  <div>我是一个Div元素</div>
  <p>我是一个段落</p>
  <div>我是一个Div元素</div>
  <p>我是一个段落</p>
  <div>我是一个Div元素</div>
  <p>我是一个段落</p>
  <div>我是一个Div元素</div>
  <p>我是一个段落</p>
</div>
```
CSS代码：
```css
.wrapper > p:nth-of-type(2n){
  background: orange;
}
```
演示结果：

![][2]


## CSS3 last-of-type选择器

“:last-of-type”选择器和“:first-of-type”选择器功能是一样的，不同的是他选择是父元素下的某个类型的最后一个子元素。

示例演示

通过“:last-of-type”选择器，将容器“div.wrapper”中最后一个段落元素背景设置为橙色

（提示：这个段落不是“div.wrapper”容器的最后一个子元素）。

HTML代码：
```html
<div class="wrapper">
  <p>我是第一个段落</p>
  <p>我是第二个段落</p>
  <p>我是第三个段落</p>
  <div>我是第一个Div元素</div>
  <div>我是第二个Div元素</div>
  <div>我是第三个Div元素</div>
</div>
```
CSS代码：
```css
 .wrapper > p:last-of-type{
  background: orange;
}
```

演示结果：

![][3]


## CSS3 nth-last-of-type(n)选择器

“:nth-last-of-type(n)”选择器和“:nth-of-type(n)”选择器是一样的，选择父元素中指定的某种子元素类型，但它的起始方向是从最后一个子元素开始，而且它的使用方法类似于上节中介绍的“:nth-last-child(n)”选择器一样。

示例演示

通过“:nth-last-of-type(n)”选择器将容器“div.wrapper”中的倒数第三个段落背景设置为橙色。

HTML代码：
```html
<div class="wrapper">
  <p>我是第一个段落</p>
  <p>我是第二个段落</p>
  <p>我是第三个段落</p>
  <p>我是第四个段落</p>
  <p>我是第五个段落</p>
  <div>我是一个Div元素</div>
  <p>我是第六个段落</p>
  <p>我是第七个段落</p>
</div>
```
CSS代码：
```css
.wrapper > p:nth-last-of-type(3){
  background: orange;
}
```
演示结果：

![][4]



## CSS3 only-child选择器

“:only-child”选择器选择的是父元素中只有一个子元素，而且只有唯一的一个子元素。也就是说，匹配的元素的父元素中仅有一个子元素，而且是一个唯一的子元素。

示例演示

通过“:only-child”选择器，来控制仅有一个子元素的背景样式，为了更好的理解，我们这个示例通过对比的方式来向大家演示。

HTML代码：
```html
<div class="post">
  <p>我是一个段落</p>
  <p>我是一个段落</p>
</div>
<div class="post">
  <p>我是一个段落</p>
</div>
```
CSS代码：
```css
.post p {
  background: green;
  color: #fff;
  padding: 10px;
}
.post p:only-child {
  background: orange;
}
```
演示结果：

![][5]


## CSS3 only-of-type选择器

“:only-of-type”选择器用来选择一个元素是它的父元素的唯一一个相同类型的子元素。这样说或许不太好理解，换一种说法。“:only-of-type”是表示一个元素他有很多个子元素，而其中只有一种类型的子元素是唯一的，使用“:only-of-type”选择器就可以选中这个元素中的唯一一个类型子元素。

示例演示

通过“:only-of-type”选择器来修改容器中仅有一个div元素的背景色为橙色。

HTML代码：
```html
<div class="wrapper">
  <p>我是一个段落</p>
  <p>我是一个段落</p>
  <p>我是一个段落</p>
  <div>我是一个Div元素</div>
</div>
<div class="wrapper">
  <div>我是一个Div</div>
  <ul>
    <li>我是一个列表项</li>
  </ul>
  <p>我是一个段落</p>
</div>
```
CSS代码：
```css
.wrapper > div:only-of-type {
  background: orange;
}
```
演示结果：

![][6]


  [1]: http://img.mukewang.com/532959820001ab6804700149.jpg
  [2]: http://img.mukewang.com/532be1ed00010f5403290494.jpg
  [3]: http://img.mukewang.com/53295ba70001d68d02470183.jpg
  [4]: http://img.mukewang.com/53295c5900015a5a02190200.jpg
  [5]: http://img.mukewang.com/53295d4a0001e07202260173.jpg
  [6]: http://img.mukewang.com/53295deb000128d801980212.jpg