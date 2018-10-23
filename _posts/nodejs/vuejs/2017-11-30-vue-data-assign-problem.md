---
title:  vue 数据 data 赋值问题
key: 20171130
tags: vue
comment: true
---

总结一下我遇到的一个纠结很久的问题。

在项目中使用了高德地图的js接口，在高德地图中绑定事件后，数据改变赋值给 vue 的 data 中的数据, 原来的代码如下：

```js
data: function() {
  return {
    center: [117.000923, 36.675807],
    lnglat: [null,null],
    mouseTool: '',
    map: ''
  }
},
mounted () {
  console.log("加载完成")

  lazyAMapApiLoaderInstance.load().then(() => {
    // your code ...
    console.log("加载地图")
    this.map = new AMap.Map('amap', {
      resizeEnable: true,
      dragEnable: true, //拖拽
      doubleClickZoom: true, //双击放大
      center: this.center,
      zoom: 12
    });

    var selfLngLat = [];
    this.map.on('click', function(e) {
      selfLngLat = [e.lnglat.lat , e.lnglat.lng];
      console.log("点击事件",e,selfLngLat)

      // 注意这行代码
      this.lnglat = selfLngLat;
      console.log("this.lnglat:" , this.lnglat)
    });
  });
}
```

在这里 `this.lnglat = selfLngLat`， `selfLngLat` 已经赋值给 `this.lnglat`，但是如图所示页面上绑定的值始终没有改变。

![mark](http://images.fuyix.cn/blog/180829/JfbLeCg5LG.png?imageslim)

![mark](http://images.fuyix.cn/blog/180829/GJi8Dial59.png?imageslim)

## 原因

原因：在请求执行成功后执行回调函数中的内容，回调函数处于其它函数的内部 `this` ，这时的 `this` 为高德地图返回的一个对象，而不是 vue 组件。

## 解决方案：

1. 将指向 vue 对象的 this 赋值给外部方法定义的属性，然后在内部方法中使用该属性

```js
var selfLngLat = [];
var _this = this;
console.log("这个this是：",this)
this.map.on('click', function(e) {
  selfLngLat = [e.lnglat.lat , e.lnglat.lng];
  console.log("点击事件",e,selfLngLat)
  console.log("这一个this是：",_this)
  // this.lnglat = selfLngLat; 这句话不正确，是因为这里的this并不是vue组件，而是高德地图返回的一个对象
  // console.log("这一个this是：",this) 使用这句话可以看到 这里的 this 到底是什么
  _this.lnglat = selfLngLat;
  console.log("this.lnglat:" , _this.lnglat)
});  
```

2. 使用箭头函数
在上面的代码中，可以看到 `lazyAMapApiLoaderInstance.load().then(() => {}` 这样一个函数，这里同样使用了回调函数，但是在里面使用this的时候是可以使用的，正是因为这里使用了箭头函数。

同样的，错误的代码我们也可以改成如下方式：

```js
this.map.on('click', (e) => {
  console.log("这一个this是：",this)
  this.lnglat = [e.lnglat.lat , e.lnglat.lng];
  console.log("点击事件", e, this.lnglat)
});
```

