---
title: CSS3 文字与字体 text-overflow 与 word-wrap
key: 20170402
tags: css3
comment: true
---

## CSS3文字与字体 text-overflow 与 word-wrap

text-overflow 用来设置是否使用一个省略标记（...）标示对象内文本的溢出。

语法：

![][1]

但是text-overflow只是用来说明文字溢出时用什么方式显示，要实现溢出时产生省略号的效果，还须定义强制文本在一行内显示（white-space:nowrap）及溢出内容为隐藏（overflow:hidden），只有这样才能实现溢出文本显示省略号的效果，代码如下：

```css
text-overflow:ellipsis; 
overflow:hidden; 
white-space:nowrap; 
```

同时，word-wrap也可以用来设置文本行为，当前行超过指定容器的边界时是否断开转行。

语法：

![][2]

normal为浏览器默认值，break-word设置在长单词或 URL地址内部进行换行，此属性不常用，用浏览器默认值即可。


  [1]: http://img.mukewang.com/53070cc00001a5bc06000200.jpg
  [2]: http://img.mukewang.com/53070cf700018a2b06000200.jpg
  
  
  ## CSS3文字与字体 嵌入字体@font-face
  
  @font-face能够加载服务器端的字体文件，让浏览器端可以显示用户电脑里没有安装的字体。

语法：

```
@font-face {
    font-family : 字体名称;
    src : 字体文件在服务器上的相对或绝对路径;
}
```
 
这样设置之后，就可以像使用普通字体一样在（font-\*）中设置字体样式。

比如：

```
p {
    font-size :12px;
    font-family : "My Font";
    /*必须项，设置@font-face中font-family同样的值*/
}
```

## CSS3文字与字体 文本阴影text-shadow

text-shadow 可以用来设置文本的阴影效果。

语法：

```
text-shadow: X-Offset Y-Offset blur color;
```

X-Offset：表示阴影的水平偏移距离，其值为正值时阴影向右偏移，反之向左偏移；      

Y-Offset：是指阴影的垂直偏移距离，如果其值是正值时，阴影向下偏移，反之向上偏移；

Blur：是指阴影的模糊程度，其值不能是负值，如果值越大，阴影越模糊，反之阴影越清晰，如果不需要阴影模糊可以将Blur值设置为0；

Color：是指阴影的颜色，其可以使用rgba色。

比如，我们可以用下面代码实现设置阴影效果。

```
text-shadow: 0 1px 1px #fff
```

