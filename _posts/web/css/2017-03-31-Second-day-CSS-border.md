---
title: CSS 边框
key: 20170331
tags: css3
comment: true
---

# css边框

## 圆角效果 border-radius

border-radius是向元素添加圆角边框。

### 使用方法：

  border-radius:10px; /* 所有角都使用半径为10px的圆角 */ 

![](http://img.mukewang.com/52e216d2000195ef01110111.jpg)

border-radius: 5px 4px 3px 2px; /* 四个半径值分别是左上角、右上角、右下角和左下角，顺时针 */ 

![](http://img.mukewang.com/52e216f9000131a201110111.jpg)

不要以为border-radius的值只能用px单位，你还可以用百分比或者em，但兼容性目前还不太好。

#### 实心上半圆：

方法：把高度(height)设为宽度（width）的一半，并且只设置左上角和右上角的半径与元素的高度一致（大于也是可以的）。
```css
div{
    height:50px;/*是width的一半*/
    width:100px;
    background:#9da;
    border-radius:50px 50px 0 0;/*半径至少设置为height的值*/
    }
```

#### 实心圆：

方法：把宽度（width）与高度(height)值设置为一致（也就是正方形），并且四个圆角值都设置为它们值的一半。如下代码：
```css
div{
    height:100px;/*与width设置一致*/
    width:100px;
    background:#9da;
    border-radius:50px;/*四个圆角值都设置为宽度或高度值的一半*/
    }
```

## 阴影 box-shadow

box-shadow是向盒子添加阴影。支持添加一个或者多个。

很简单的一段代码，就实现了投影效果，酷毙了。我们来看下语法：

    box-shadow: X轴偏移量 Y轴偏移量 [阴影模糊半径] [阴影扩展半径] [阴影颜色] [投影方式];

参数介绍：
![](http://img.mukewang.com/54292d620001ffb107080250.jpg)

注意：inset 可以写在参数的第一个或最后一个，其它位置是无效的。

### 为元素设置外阴影：

示例代码：
```css
.box_shadow{
  box-shadow:4px 2px 6px #333333; 
}
```

### 为元素设置内阴影：

示例代码：
```css
.box_shadow{
  box-shadow:4px 2px 6px #333333 inset; 
}
```

### 添加多个阴影：
以上的语法的介绍，就这么简单，如果添加多个阴影，只需用逗号隔开即可。如：
```css
.box_shadow{
    box-shadow:4px 2px 6px #f00, -4px -2px 6px #000, 0px 0px 12px 5px #33CC00 inset;
}
```

### 阴影模糊半径与阴影扩展半径的区别

阴影模糊半径：此参数可选，其值只能是为正值，如果其值为0时，表示阴影不具有模糊效果，其值越大阴影的边缘就越模糊；

阴影扩展半径：此参数可选，其值可以是正负值，如果值为正，则整个阴影都延展扩大，反之值为负值时，则缩小；

### X轴偏移量和Y轴偏移量值可以设置为负数

    box-shadow: X轴偏移量 Y轴偏移量 [阴影模糊半径] [阴影扩展半径] [阴影颜色] [投影方式];
X轴偏移量为负数：
```css
.boxshadow-outset{
    width:100px;
    height:100px;
    box-shadow:-4px 4px 6px #666;
}
```
效果图：

![](http://img.mukewang.com/548fd91c000140a901540143.jpg)

Y轴偏移量为负数：
```css
.boxshadow-outset{
    width:100px;
    height:100px;
    box-shadow:4px -4px 6px #666;
}
```
效果图：

![](http://img.mukewang.com/548fd93e00011dd101570142.jpg)


