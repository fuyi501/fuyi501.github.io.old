---
title: thinkjs 教程安装及运行
key: 20170630
tags: thinkjs
comment: true
---

## thinkjs介绍

> ThinkJS 是一款使用 ES6/7 特性全新开发的 Node.js MVC 框架，使用 ES7 中 async/await，或者 ES6 中的 */yield 特性彻底解决了 Node.js 中异步嵌套的问题。同时吸收了国内外众多框架的设计理念和思想，让开发 Node.js 项目更加简单、高效。

> 使用 ES6/7 特性来开发项目可以大大提高开发效率，是趋势所在。并且新版的 Node.js 对 ES6 特性也有了较好的支持，即使有些特性还没有支持，也可以借助 Babel 编译来支持。

[thinkjs官网][1]

## 安装thinkjs

通过下面的命令即可安装 ThinkJS：

```sh
npm install thinkjs@2 -g --verbose
```

如果安装很慢的话，可以尝试使用 taobao 的源进行安装。具体如下：

```sh
npm install thinkjs@2 -g --registry=https://registry.npm.taobao.org --verbose
```

安装完成后，可以通过` thinkjs --version` 或 `thinkjs -V `命令查看安装的版本。

## 创建项目

ThinkJS 安装完成后，就可以通过下面的命令创建项目:

```sh
thinkjs new project_path; # project_path 为项目存放的目录
```

## 安装依赖

项目安装后，进入项目目录，执行 npm install 安装依赖，可以使用 taobao 源进行安装。

```sh
npm install --registry=https://registry.npm.taobao.org --verbose
```

## 启动项目

在项目目录下执行命令 

```sh	
npm start
```

出现以下信息表示运行成功

```
[2017-04-10 15:00:33] [THINK] Server running at http://127.0.0.1:8360/
[2017-04-10 15:00:33] [THINK] ThinkJS Version: 2.2.18
[2017-04-10 15:00:33] [THINK] Cluster Status: closed
[2017-04-10 15:00:33] [THINK] WebSocket Status: closed
[2017-04-10 15:00:33] [THINK] File Auto Compile: true
[2017-04-10 15:00:33] [THINK] File Auto Reload: true
[2017-04-10 15:00:33] [THINK] App Enviroment: development
```

## 访问项目

打开浏览器，访问` http://127.0.0.1:8360/ ` 即可。

![thinkjs运行图][2]


## 更改端口号

打开` /src/common/config/config.js ` 文件，设置端口 为9000，访问超时为 30s

```js
'use strict';
/**
 * config
 */
export default {
  //key: value
  port: 9000, //将监听的端口修改为 9000
  timeout: 30 //将超时时间修改为 30s
};

```

## 修改模板引擎

打开` /src/common/config/view.js ` 文件，默认的模板引擎是 ` ejs `，可以更改成` nunjucks `

```js
'use strict';
/**
 * template config
 */
export default {
  type: 'ejs',
  content_type: 'text/html',
  file_ext: '.html',
  file_depr: '_',
  root_path: think.ROOT_PATH + '/view',
  adapter: {
    ejs: {}
  }
};
```

修改模板引擎为 `nunjucks` 

```js
'use strict';
/**
 * template config
 */
export default {
  type: 'nunjucks',
  content_type: 'text/html',
  file_ext: '.html',
  file_depr: '_',
  root_path: think.ROOT_PATH + '/view',
  adapter: {
    nunjucks: {}
  }
};
```
## Hello 展示

上面的设置完成了，基本框架就完成了，下面来看看如何进行页面修改和展示。

总的来说，thinkjs 使用还是很简单的，只需要对应控制器和视图就可以，详细内容请看[官网介绍](1)，下面来简单介绍下，修改控制器内容并在前端展示。

打开 ` src/home/controler/index.js ` 文件，添加一句话 

```js
'use strict';

import Base from './base.js';

export default class extends Base {
  /**
   * index action
   * @return {Promise} []
   */
  indexAction(){
    //auto render template file index_index.html
    this.assign("title","Hello Thinkjs") //给title赋值为 Hello Thinkjs
    return this.display();
  }
}
```

再打开 `view/home/index_index.html` 文件，再 `<h1>` 下面添加 

```html
<div class="wrap">
      <h1>A New App Created By ThinkJS</h1>
      <h2>{{ title }}</h2>
</div>
```

为了显示好看，设置`<h2>` 的css样式如下：

```css
h2 { font-size: 26px; color:#fff; font-weight: normal; margin-top: 40px; }
```

这时重启 项目 `npm start` ，输出如下：

```
[2017-04-10 15:49:15] [THINK] Server running at http://127.0.0.1:9000/
[2017-04-10 15:49:15] [THINK] ThinkJS Version: 2.2.18
[2017-04-10 15:49:15] [THINK] Cluster Status: closed
[2017-04-10 15:49:15] [THINK] WebSocket Status: closed

[2017-04-10 15:49:15] [THINK] File Auto Compile: true
[2017-04-10 15:49:15] [THINK] File Auto Reload: true
[2017-04-10 15:49:15] [THINK] App Enviroment: development
 
 ```
 
 端口已经变成了 `9000`，浏览器打开 ` http://localhost:9000/ `，发现 `Hello Thinkjs` 已经显示出来了，如下：
 
 ![enter description here][3]
 
 ## 总结
 
 thinkjs 基本使用方式就完成了，从上面可以看到，使用 `thinkjs` 只需要操作 控制器 和 视图 就可以了。当然，这样可能并不能满足我们的需求，复杂点的看后续博客。


  [1]: https://thinkjs.org/zh-cn/doc/index.html
  [2]: http://images.fuyix.cn/thinkjsrun.png "thinkjsrun"
  [3]: http://images.fuyix.cn/hellothinkjs.png "hellothinkjs"