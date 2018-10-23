---
title: CSS3 背景 background-origin
key: 20170403
tags: css3
comment: true
---

## CSS3背景 background-origin

设置元素背景图片的原始起始位置。

语法：

```
background-origin ： border-box | padding-box | content-box;
```

参数分别表示背景图片是从边框，还是内边距（默认值），或者是内容区域开始显示。

效果如下：

![][1]

需要注意的是，如果背景不是no-repeat，这个属性无效，它会从边框开始显示。

## CSS3背景 background-clip

用来将背景图片做适当的裁剪以适应实际需要。

语法：

```
background-clip ： border-box | padding-box | content-box | no-clip
```

参数分别表示从边框、或内填充，或者内容区域向外裁剪背景。no-clip表示不裁切，和参数border-box显示同样的效果。backgroud-clip默认值为border-box。

效果如下图所示：

![][2]

## CSS3背景 background-size

设置背景图片的大小，以长度值或百分比显示，还可以通过cover和contain来对图片进行伸缩。

语法：
```
background-size: auto | <长度值> | <百分比> | cover | contain
```

取值说明：

1、auto：默认值，不改变背景图片的原始高度和宽度；

2、<长度值>：成对出现如200px 50px，将背景图片宽高依次设置为前面两个值，当设置一个值时，将其作为图片宽度值来等比缩放；

3、<百分比>：0％~100％之间的任何值，将背景图片宽高依次设置为所在元素宽高乘以前面百分比得出的数值，当设置一个值时同上；

4、cover：顾名思义为覆盖，即将背景图片等比缩放以填满整个容器；

5、contain：容纳，即将背景图片等比缩放至某一边紧贴容器边缘为止。


## CSS3背景 multiple backgrounds

多重背景，也就是CSS2里background的属性外加origin、clip和size组成的新background的多次叠加，缩写时为用逗号隔开的每组值；用分解写法时，如果有多个背景图片，而其他属性只有一个（例如background-repeat只有一个），表明所有背景图片应用该属性值。

语法缩写如下：
```css
background ： [background-color] | [background-image] | [background-position][/background-size] | [background-repeat] | [background-attachment] | [background-clip] | [background-origin],...
```

可以把上面的缩写拆解成以下形式：

```css
background-image:url1,url2,...,urlN;

background-repeat : repeat1,repeat2,...,repeatN;
backround-position : position1,position2,...,positionN;
background-size : size1,size2,...,sizeN;
background-attachment : attachment1,attachment2,...,attachmentN;
background-clip : clip1,clip2,...,clipN;
background-origin : origin1,origin2,...,originN;
background-color : color;
```

注意：

1. 用逗号隔开每组 background 的缩写值；
2. 如果有 size 值，需要紧跟 position 并且用 "/" 隔开；
3. 如果有多个背景图片，而其他属性只有一个（例如 background-repeat 只有一个），表明所有背景图片应用该属性值。
4. background-color 只能设置一个。

举例：

有三张单独的图片：

![][3]

![][4]

![][5]

使用多背景技术实现：

![][6]

具体代码实现:

```css
.demo{
    width: 300px;
    height: 140px;
    border: 1px solid #999;
    
    background-image: url(http://img.mukewang.com/54cf2365000140e600740095.jpg),
                      url(http://img.mukewang.com/54cf238a0001728d00740095.jpg),
                      url(http://img.mukewang.com/54cf23b60001fd9700740096.jpg);
    background-position: left top, 100px 0, 200px 0;
    background-repeat: no-repeat, no-repeat, no-repeat;
    
    margin:0 0 20px 0;
}
```


  [1]: http://img.mukewang.com/531003de0001166903660166.jpg
  [2]: http://img.mukewang.com/5310065d0001c95103660166.jpg
  [3]: http://img.mukewang.com/54cf2365000140e600740095.jpg
  [4]: http://img.mukewang.com/54cf238a0001728d00740095.jpg
  [5]: http://img.mukewang.com/54cf23b60001fd9700740096.jpg
  [6]: http://img.mukewang.com/54cf289200013de502790094.jpg