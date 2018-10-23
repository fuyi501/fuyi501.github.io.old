---
title: CSS3 结构性伪类选择器 root
key: 20170404-02
tags: css3
comment: true
---

## CSS3 结构性伪类选择器—root
`:root` 选择器，从字面上我们就可以很清楚的理解是根选择器，他的意思就是匹配元素E所在文档的根元素。在HTML文档中，根元素始终是`<html>`。

示例演示：

通过 “:root” 选择器设置背景颜色

HTML代码：
```
<div>:root选择器的演示</div>
```
CSS代码：
```
:root {
  background:orange;
}
```
演示结果：

![][1]

“:root”选择器等同于`<html>`元素，简单点说：

```
:root{background:orange}

html {background:orange;}
```
得到的效果等同。

建议使用:root方法。

## CSS3 结构性伪类选择器—not

`:not` 选择器称为否定选择器，和jQuery中的:not选择器一模一样，可以选择除某个元素之外的所有元素。就拿form元素来说，比如说你想给表单中除submit按钮之外的input元素添加红色边框
CSS代码可以写成：
```css
form {
  width: 200px;
  margin: 20px auto;
}
div {
  margin-bottom: 20px;
}
input:not([type="submit"]){
  border:1px solid red;
}
```
相关HTML代码：

```html
<form action="#">
  <div>
    <label for="name">Text Input:</label>
    <input type="text" name="name" id="name" placeholder="John Smith" />
  </div>
  <div>
    <label for="name">Password Input:</label>
    <input type="text" name="name" id="name" placeholder="John Smith" />
  </div>
  <div>
    <input type="submit" value="Submit" />
  </div>
</form>  
```

演示结果：

![][2]


## CSS3 结构性伪类选择器—empty

`:empty` 选择器表示的就是空。用来选择没有任何内容的元素，这里没有内容指的是一点内容都没有，哪怕是一个空格。

示例显示：

比如说，你的文档中有三个段落p元素，你想把没有任何内容的P元素隐藏起来。我们就可以使用“:empty”选择器来控制。

HTML代码：
```html
<p>我是一个段落</p>
<p> </p>
<p></p>
```
CSS代码：
```css
p{
 background: orange;
 min-height: 30px;
}
p:empty {
  display: none;
}
```

演示结果：

![][3]

## CSS3 结构性伪类选择器—target

`:target` 选择器称为目标选择器，用来匹配文档(页面)的url的某个标志符的目标元素。我们先来上个例子，然后再做分析。

示例展示

点击链接显示隐藏的段落。

HTML代码：
```html
<h2><a href="#brand">Brand</a></h2>
<div class="menuSection" id="brand">
    content for Brand
</div>
```
CSS代码：
```css
.menuSection{
  display: none;
}
:target{/*这里的:target就是指id="brand"的div对象*/
  display:block;
}
```
演示结果：

![][4]

分析：

1、具体来说，触发元素的URL中的标志符通常会包含一个#号，后面带有一个标志符名称，上面代码中是：#brand

2、：target就是用来匹配id为“brand”的元素（id="brand"的元素）,上面代码中是那个div元素。

多个url（多个target）处理：

就像上面的例子，#brand与后面的id="brand"是对应的，当同一个页面上有很多的url的时候你可以取不同的名字，只要#号后对的名称与id=""中的名称对应就可以了。
如下面例子：
html代码：  

```html
<h2><a href="#brand">Brand</a></h2>
<div class="menuSection" id="brand">
  content for Brand
</div>
<h2><a href="#jake">Brand</a></h2>
<div class="menuSection" id="jake">
 content for jake
</div>
<h2><a href="#aron">Brand</a></h2>
<div class="menuSection" id="aron">
    content for aron
</div>
```

css代码：

```css
#brand:target {
  background: orange;
  color: #fff;
}
#jake:target {
  background: blue;
  color: #fff;
}
#aron:target {
  background: red;
  color: #fff;
}
```
上面的代码可以对不同的target对象分别设置不的样式。


## CSS3 结构性伪类选择器—first-child

`:first-child` 选择器表示的是选择父元素的第一个子元素的元素E。简单点理解就是选择元素中的第一个子元素，记住是子元素，而不是后代元素。

示例演示

通过“:first-child”选择器定位列表中的第一个列表项，并将序列号颜色变为红色。

HTML代码：
```html
<ol>
  <li><a href="##">Link1</a></li>
  <li><a href="##">Link2</a></li>
  <li><a href="##">link3</a></li>
</ol>
```
CSS代码：
```css
ol > li{
  font-size:20px;
  font-weight: bold;
  margin-bottom: 10px;
}

ol a {
  font-size: 16px;
  font-weight: normal;
}

ol > li:first-child{
  color: red;
}
```
演示结果：

![][5]

## CSS3 结构性伪类选择器—last-child

`:last-child` 选择器与“:first-child”选择器作用类似，不同的是“:last-child”选择器选择的是元素的最后一个子元素。例如，需要改变的是列表中的最后一个“li”的背景色，就可以使用这个选择器，

	ul>li:last-child{background:blue;}

示例演示

在博客的排版中，每个段落都有15px的margin-bottom，假设不想让博客“post”中最后一个段落不需要底部的margin值，可以使用“:last-child”选择器。

HTML代码：
```html
<div class="post">
  <p>第一段落</p>
  <p>第二段落</p>
  <p>第三段落</p>
  <p>第四段落</p>
  <p>第五段落</p>
</div>
```
CSS代码：
```css
.post {
  padding: 10px;
  border: 1px solid #ccc;
  width: 200px;
  margin: 20px auto;
}
.post p {
  margin:0 0 15px 0;
}

.post p:last-child {
  margin-bottom:0;
}
```
演示结果：

![][6]


## CSS3 结构性伪类选择器—nth-child(n)

`:nth-child(n)` 选择器用来定位某个父元素的一个或多个特定的子元素。其中“n”是其参数，而且可以是整数值(1,2,3,4)，也可以是表达式(2n+1、-n+5)和关键词(odd、even)，但参数n的起始值始终是1，而不是0。也就是说，参数n的值为0时，选择器将选择不到任何匹配的元素。

经验与技巧:当“:nth-child(n)”选择器中的n为一个表达式时，其中n是从0开始计算，当表达式的值为0或小于0的时候，不选择任何匹配的元素。如下表所示：

![][7]

案例演示

  通过“:nth-child(n)”选择器，并且参数使用表达式“2n”，将偶数行列表背景色设置为橙色。

HTML代码：
```html
<ol>
  <li>item1</li>
  <li>item2</li>
  <li>item3</li>
  <li>item4</li>
  <li>item5</li>
  <li>item6</li>
  <li>item7</li>
  <li>item8</li>
  <li>item9</li>
  <li>item10</li>
</ol>
```
CSS代码：
```css
ol > li:nth-child(2n){
  background: orange;
}
```
演示结果：

![][8]


## CSS3 结构性伪类选择器—nth-last-child(n)

`:nth-last-child(n)` 选择器和前面的“:nth-child(n)”选择器非常的相似，只是这里多了一个“last”，所起的作用和“:nth-child(n)”选择器有所区别，从某父元素的最后一个子元素开始计算，来选择特定的元素。

案例演示

选择列表中倒数第五个列表项，将其背景设置为橙色。

HTML代码：
```html
<ol>
  <li>item1</li>
  <li>item2</li>
  <li>item3</li>
  <li>item4</li>
  <li>item5</li>
  <li>item6</li>
  <li>item7</li>
  <li>item8</li>
  <li>item9</li>
  <li>item10</li>
  <li>item11</li>
  <li>item12</li>
  <li>item13</li>
  <li>item14</li>
  <li>item15</li>
</ol>
```
CSS代码：
```css
ol > li:nth-last-child(5){
  background: orange;
}
```
演示结果：

![][9]


  [1]: http://img.mukewang.com/531eb1d90001735d01560050.jpg
  [2]: http://img.mukewang.com/531eb2ca00014b5002370177.jpg
  [3]: http://img.mukewang.com/531eb55b0001d7d401580126.jpg
  [4]: http://img.mukewang.com/53217ea30001103002230101.jpg
  [5]: http://img.mukewang.com/531eb8ca0001c5ba01250125.jpg
  [6]: http://img.mukewang.com/531eb9cb0001428002760188.jpg
  [7]: http://img.mukewang.com/531eba56000138aa05870197.jpg
  [8]: http://img.mukewang.com/531ebac90001750902580228.jpg
  [9]: http://img.mukewang.com/531ebb7a0001d20b01530286.jpg