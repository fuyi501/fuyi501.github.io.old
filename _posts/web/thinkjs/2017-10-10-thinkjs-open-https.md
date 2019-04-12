---
title:  thinkjs 开启 https 服务
key: 20171010
tags: thinkjs https
comment: true
---

> 在使用 ThinkJS 的时候一般都会搭配 Nginx 使用，在 Nginx 中配置 HTTPS 是非常简单的。这样做的原理是 Nginx 接收到 HTTPS 的请求后反向代理到 ThinkJS 服务的端口上，从而达到了 ThinkJS 项目开启 HTTPS 服务的目的。

不过有时候使用 ThinkJS 作为后台服务器设计 API 的时候，请求也是 https，这时使用 Nginx 代理就不行了，具体出错的问题由于没有保存所以找不到了，有时间了再重现一下。这个时候就需要使用 ThinkJS 启动自己的 HTTPS 服务了。

> 其实方法非常的简单，虽然 ThinkJS 默认是使用 NodeJS 的 http 模块启动服务的，但是作者也同时开放了接口支持用户自定义 server。config.js 中提供了 create_server 属性来自定义启动服务：

> create_server: undefined, //自定义启动服务

那么具体如何定义这个属性呢，具体我们可以从源码中窥知一二：

```js
static createServer(){
  let handle = think.config('create_server');
  let host = think.config('host');
  let port = think.port || think.config('port'); 
  // createServer callback
  let callback = (req, res) => {
    think.http(req, res).then(http => {
      new this(http).run();
    });
  };
  let server;
  // define createServer in application
  if (handle) {
    server = handle(callback, port, host, this);
  }else{
    //create server
    server = http.createServer(callback);
    server.listen(port, host);
  }
  think.server = server;
  // start websocket
  let websocket = think.parseConfig(think.config('websocket'));
  if(websocket.on){
    let Cls = think.adapter('websocket', websocket.type);
    let instance = new Cls(server, websocket, this);
    instance.run();
  }
}
```

从源码中可以看到，ThinkJS 会将监听回调，监听端口，监听 HOST 当做参数传给我们的自定义启动服务变量，所以我们只要按照给定的参数定义好启动服务函数就可以了：

```js
/**
 * src/common/config.js 
**/
import fs from 'fs';
import https from 'https';
const options = {
  key: fs.readFileSync('./ssl/server.key'),
  cert: fs.readFileSync('./ssl/server.crt')
};
const app = (callback, port, host, think) => {
  let server = https.createServer(options, callback);
  server.listen(port, host);
  return server;
}
export default {
  create_server: app
};
```

其中 `server.key` 和 `server.crt` 是下发的证书文件，可以是 SSL 供应商下发的证书，也可以是本地自签名的证书。

这样启动 ThinkJS 之后就可以使用 `https://localhost:8360` 访问网站了，这里需要注意的是如果是自签名证书的话因为是不受信任的 CA 所以浏览器会报不安全的链接，如果是真实 CA 下发的如果签发域名和访问域名不一致的话也会报不安全的链接。

## 参考

[https://imnerd.org/thinkjs-create-https-server.html](https://imnerd.org/thinkjs-create-https-server.html)