---
title: CSS3 选择器-其他2
key: 20170405-02
tags: css3
comment: true
---

## CSS3选择器 :enabled选择器

在Web的表单中，有些表单元素有可用（“:enabled”）和不可用（“:disabled”）状态，比如输入框，密码框，复选框等。在默认情况之下，这些表单元素都处在可用状态。那么我们可以通过伪选择器“:enabled”对这些表单元素设置样式。

示例演示

通过“:enabled”选择器，修改文本输入框的边框为2像素的红色边框，并设置它的背景为灰色。

HTML代码:
```html
<form action="#">
  <div>
    <label for="name">Text Input:</label>
    <input type="text" name="name" id="name" placeholder="可用输入框"  />
  </div>
   <div>
    <label for="name">Text Input:</label>
    <input type="text" name="name" id="name" placeholder="禁用输入框"  disabled="disabled" />
  </div>
</form>  
```
CSS代码：
```css
div{
  margin: 20px;
}
input[type="text"]:enabled {
  background: #ccc;
  border: 2px solid red;
}
```
结果演示

![][1]


## CSS3选择器 :disabled选择器

“:disabled”选择器刚好与“:enabled”选择器相反，用来选择不可用表单元素。要正常使用“:disabled”选择器，需要在表单元素的HTML中设置“disabled”属性。

示例演示

通过“:disabled”选择器，给不可用输入框设置明显的样式。

HTML代码：
```html
<form action="#">
  <div>
    <input type="text" name="name" id="name" placeholder="我是可用输入框" />
  </div>
  <div>
    <input type="text" name="name" id="name" placeholder="我是不可用输入框" disabled />
  </div>
</form>  
```
CSS代码
```css
form {
  margin: 50px;
}
div {
  margin-bottom: 20px;
}
input {
  background: #fff;
  padding: 10px;
  border: 1px solid orange;
  border-radius: 3px;
}
input[type="text"]:disabled {
  background: rgba(0,0,0,.15);
  border: 1px solid rgba(0,0,0,.15);
  color: rgba(0,0,0,.15);
}
```
结果演示：

![][2]


## CSS3选择器 :checked选择器

## CSS3选择器 ::selection选择器

“::selection”伪元素是用来匹配突出显示的文本(用鼠标选择文本时的文本)。浏览器默认情况下，用鼠标选择网页文本是以“深蓝的背景，白色的字体”显示的，效果如下图所示：

![][3]

从上图中可以看出，用鼠标选中“专注IT、互联网技术”、“纯干货、学以致用”、“没错、这是免费的”这三行文本中，默认显示样式为：蓝色背景、白色文本。

有的时候设计要求,不使用上图那种浏览器默认的突出文本效果，需要一个与众不同的效果，此时“::selection”伪元素就非常的实用。不过在Firefox浏览器还需要添加前缀。

示例演示:

通过“::selection”选择器，将Web中选中的文本背景变成红色，文本变成绿色。

HTML代码：
```html
<p>“::selection”伪元素是用来匹配突出显示的文本。浏览器默认情况下，选择网站文本是深蓝的背景，白色的字体，</p>
```
CSS代码：
```css
::-moz-selection {
  background: red;
  color: green;
}
::selection {
  background: red;
  color: green;
}
```
结果演示：

![][4]

注意：

1、IE9+、Opera、Google Chrome 以及 Safari 中支持 ::selection 选择器。

2、Firefox 支持替代的 ::-moz-selection。

## CSS3选择器 :read-only选择器

## CSS3选择器 :read-write选择器

## CSS3选择器 ::before和::after


  [1]: http://img.mukewang.com/5335299700015e3403000084.jpg
  [2]: http://img.mukewang.com/53326c450001881402490139.jpg
  [3]: http://img.mukewang.com/533e50910001bff808670219.jpg
  [4]: http://img.mukewang.com/533e52b80001973007260033.jpg