---
title:  服务器配置 ssl
key: key-20180110
tags: 服务器 ssl
comment: true
---

## 本地测试环境配置 https

### https 简介

1. https简介
HTTPS其实是有两部分组成：HTTP + SSL / TLS，也就是在HTTP上又加了一层处理加密信息的模块。服务端和客户端的信息传输都会通过TLS进行加密，所以传输的数据都是加密后的数据

2. https协议原理
首先，客户端与服务器建立连接，各自生成私钥和公钥，是不同的。服务器返给客户端一个公钥，然后客户端拿着这个公钥把要搜索的东西加密，称之为密文，并连并自己的公钥一起返回给服务器，服务器拿着自己的私钥解密密文，然后把响应到的数据用客户端的公钥加密，返回给客户端，客户端拿着自己的私钥解密密文，把数据呈现出来

### 证书和私钥生成

1. 创建服务器证书密钥文件 server.key：

`openssl genrsa -des3 -out server.key 1024`

输入密码，确认密码，自己随便定义，但是要记住，后面会用到。

2. 创建服务器证书的申请文件 server.csr

`openssl req -new -key server.key -out server.csr`

输出内容为：

```sh
Enter pass phrase for root.key: ← 输入前面创建的密码 

Country Name (2 letter code) [AU]:CN ← 国家代号，中国输入CN 

State or Province Name (full name) [Some-State]:BeiJing ← 省的全名，拼音 

Locality Name (eg, city) []:BeiJing ← 市的全名，拼音 

Organization Name (eg, company) [Internet Widgits Pty Ltd]:MyCompany Corp. ← 公司英文名 

Organizational Unit Name (eg, section) []: ← 可以不输入 

Common Name (eg, YOUR name) []: ← 此时不输入 

Email Address []:admin@mycompany.com ← 电子邮箱，可随意填

Please enter the following ‘extra’ attributes 

to be sent with your certificate request 

A challenge password []: ← 可以不输入 

An optional company name []: ← 可以不输入
```

4. 备份一份服务器密钥文件

`cp server.key server.key.org`

5. 去除文件口令

`openssl rsa -in server.key.org -out server.key`

6. 生成证书文件server.crt

`openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt`

### 配置 nginx 服务器

1. 下面为配置文件 `/etc/nginx/conf.d/xxx.conf`

```bash
server{
    listen 443 default ssl;  
    # 比起默认的80 使用了443 默认 是ssl方式  多出default之后的ssl
    # default 可省略
    # 开启  如果把ssl on；这行去掉，ssl写在443端口后面。这样http和https的链接都可以用
    # ssl on;
    server_name localhost; # 绑定域名

    ssl_certificate ssl/server.crt; # 证书(公钥.发送到客户端的)
    ssl_certificate_key ssl/server.key; # 私钥
    
    location / {

    proxy_redirect off; # 禁止跳转

    }        
}
```

## 问题

### This request has been blocked; the content must be served over HTTPS.

HTTPS 是 HTTP over Secure Socket Layer，以安全为目标的 HTTP 通道，所以在 HTTPS 承载的页面上不允许出现 http 请求，一旦出现就是提示或报错：

Mixed Content: The page at ‘https://www.taobao.com/‘ was loaded over HTTPS, but requested an insecure image ‘http://g.alicdn.com/s.gif’. This content should also be served over HTTPS.

CSP 设置 upgrade-insecure-requests

好在 W3C 工作组考虑到了我们升级 HTTPS 的艰难，在 2015 年 4 月份就出了一个 Upgrade Insecure Requests 的草案（http://www.w3.org/TR/mixed-content/），他的作用就是让浏览器自动升级请求。

在我们服务器的响应头中加入：

header("Content-Security-Policy: upgrade-insecure-requests");

我们的页面是 https 的，而这个页面中包含了大量的 http 资源（图片、iframe等），页面一旦发现存在上述响应头，会在加载 http 资源时自动替换成 https 请求。可以查看 google 提供的一个 demo（https://googlechrome.github.io/samples/csp-upgrade-insecure-requests/index.html）：

当然，如果我们不方便在服务器 Nginx 上操作，也可以在页面中加入 meta 头：

```html
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests" />
```

从 W3C 工作组给出的 example（http://www.w3.org/TR/upgrade-insecure-requests/#examples），可以看出，这个设置不会对外域的 a 链接做处理，所以可以放心使用。
