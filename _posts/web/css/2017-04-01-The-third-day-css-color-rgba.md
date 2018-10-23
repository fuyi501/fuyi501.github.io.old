---
title: CSS3 颜色之 RGBA
key: 20170401
tags: css3
comment: true
---

# CSS3颜色之RGBA

RGB是一种色彩标准，是由红(R)、绿(G)、蓝(B)的变化以及相互叠加来得到各式各样的颜色。RGBA是在RGB的基础上增加了控制alpha透明度的参数。

语法：

  color：rgba(R,G,B,A)

以上R、G、B三个参数，正整数值的取值范围为：0 - 255。百分数值的取值范围为：0.0% - 100.0%。超出范围的数值将被截至其最接近的取值极限。并非所有浏览器都支持使用百分数值。A为透明度参数，取值在0~1之间，不可为负值。

代码示例：
```css
background-color:rgba(100,120,60,0.5);
background-color:rgba(255,255,255,0.5);
```

## CSS3颜色 渐变色彩 

CSS3 Gradient 分为线性渐变(linear)和径向渐变(radial)。由于不同的渲染引擎实现渐变的语法不同，这里我们只针对线性渐变的 W3C 标准语法来分析其用法，其余大家可以查阅相关资料。W3C 语法已经得到了 IE10+、Firefox19.0+、Chrome26.0+ 和 Opera12.1+等浏览器的支持。

这一小节我们来说一下线性渐变：

![](http://img.mukewang.com/54b72b2e0001500103790158.jpg)

参数：

第一个参数:指定渐变方向，可以用“角度”的关键词或“英文”来表示：

![](http://img.mukewang.com/542a25da00017e9406980223.jpg)

第一个参数省略时，默认为“180deg”，等同于“to bottom”。

第二个和第三个参数，表示颜色的起始点和结束点，可以有多个颜色值。
```css
background-image:linear-gradient(to left, red, orange,yellow,green,blue,indigo,violet);
```
效果图：
![](http://img.mukewang.com/54b72c080001611c04230192.jpg)

