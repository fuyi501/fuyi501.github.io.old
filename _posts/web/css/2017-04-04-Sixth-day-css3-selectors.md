---
title: CSS3 选择器 属性选择器
key: 20170404-01
tags: css3
comment: true
---

## CSS3选择器 属性选择器

在HTML中，通过各种各样的属性可以给元素增加很多附加的信息。例如，通过id属性可以将不同div元素进行区分。

   在CSS2中引入了一些属性选择器，而CSS3在CSS2的基础上对属性选择器进行了扩展，新增了3个属性选择器，使得属性选择器有了通配符的概念，这三个属性选择器与CSS2的属性选择器共同构成了CSS功能强大的属性选择器。如下表所示：

![][1]

 实例展示：

html代码：

```html
<a href="xxx.pdf">我链接的是PDF文件</a>
<a href="#" class="icon">我类名是icon</a>
<a href="#" title="我的title是more">我的title是more</a>
```
css代码  
```css
a[class^=icon]{
  background: green;
  color:#fff;
}
a[href$=pdf]{
  background: orange;
  color: #fff;
}
a[title*=more]{
  background: blue;
  color: #fff;
}
```

 结果显示：

![][2]


  [1]: http://img.mukewang.com/56653eba0001b07004610215.jpg
  [2]: http://img.mukewang.com/53202e050001c07e03820068.jpg