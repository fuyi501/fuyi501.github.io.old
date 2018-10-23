---
title: nginx 服务器配置方法
key: 20160706
tags: nginx 服务器
comment: true
---

## Nginx 服务器的配置

Nginx 服务器的使用实际上重点就在于它的配置，作为代理服务器，你可以在服务器上启动多个应用，由 Nginx 配置多个域名，实现服务的代理。

服务器系统：ubuntu server 16.04
nginx 版本号：nginx/1.10.0 (Ubuntu)

登录服务器，进入到 Nginx 的安装目录 `/etc/nginx/`,可以看到很多文件

```sh
ubuntu@VM-176-3-ubuntu:~$ cd /etc/nginx/
ubuntu@VM-176-3-ubuntu:/etc/nginx$ ls
conf.d          koi-utf     nginx.conf    sites-available  uwsgi_params
fastcgi.conf    koi-win     proxy_params  sites-enabled    win-utf
fastcgi_params  mime.types  scgi_params   snippets
```

其中，`sites-enabled` 中存放着 nginx 的默认配置文件， `conf.d` 目录存放其他的配置文件，不过目前 `conf.d` 目录下是空的，以后可以将自定义的配置文件都放到这个目录下。

```sh
ubuntu@VM-176-3-ubuntu:/etc/nginx$ ls sites-enabled/
default
ubuntu@VM-176-3-ubuntu:/etc/nginx$ ls conf.d/
ubuntu@VM-176-3-ubuntu:/etc/nginx$ 
```

在 nginx 目录下有一个 `nginx.conf` 配置文件，里面有这样两句代码：

```
include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;
```

我们将配置文件放在 `conf.d` 目录后，使用命令 `sudo nginx -s reload` ，就会重新加载里面所有的配置文件。

下面用两个例子来验证 nginx 服务器的代理转发

一个是 thinkjs 的测试项目，占用端口 8360；一个是 hexo 的测试项目，占用端口4000.

首先在用户目录下下载安装这两个项目，下载好了如下：

```bash
ubuntu@VM-176-3-ubuntu:~$ ls
blog_server  project_test
```

thinkjs 测试项目自己带有 nginx.conf 配置文件，我们只需要将这个配置文件复制到 `/etc/nginx/conf.d` 目录下就可以了，详情请参考官方文档的 [使用 nginx 做反向代理](https://thinkjs.org/zh-cn/doc/2.2/deploy.html#toc-c69)配置多个域名，实现服务的代理。

hexo 测试项目因为里面没有 index.html 文件，所以要先 执行 `hexo g` 来生成静态文件，执行之后会生成一个 public 目录，public 目录下就会有 index.html 文件，然后我们需要为 hexo 项目写一个 conf 文件，内容类似如下：

```
server {
    listen 80;
    server_name blog.fuyix.cn;
    set $node_port 4000;

    location / {
	    proxy_pass http://localhost:$node_port;
    }
}
```

注意：将 `server_name localhost` 里的 `localhost` 修改为对应的域名，将 `set $node_port 4000` 里的 `4000` 修改和项目里监听的端口一致。我这里使用默认的 `4000` 端口。

然后在 `/etc/nginx/conf.d` 目录下新建 hexo_nginx.conf 文件，拷贝上面的代码粘贴在里面，然后保存退出，重新加载 nginx 的配置文件就可以了，如下：

```bash
ubuntu@VM-176-3-ubuntu:/etc/nginx/conf.d$ ls
thinkjs_nginx.conf
ubuntu@VM-176-3-ubuntu:/etc/nginx/conf.d$ cat thinkjs_nginx.conf
server {
    listen 80;
    server_name think.fuyix.cn;
    root /home/ubuntu/project_test/www;
    set $node_port 8360;

    index index.js index.html index.htm;
    if ( -f $request_filename/index.html ){
        rewrite (.*) $1/index.html break;
    }
    if ( !-f $request_filename ){
        rewrite (.*) /index.js;
    }
    location = /index.js {
        proxy_http_version 1.1;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_pass http://127.0.0.1:$node_port$request_uri;
        proxy_redirect off;
    }
    
    location = /development.js {
        deny all;
    }

    location = /testing.js {
        deny all;
    }

    location = /production.js {
        deny all;
    }

    location ~ /static/ {
        etag         on;
        expires      max;
    }
}   
ubuntu@VM-176-3-ubuntu:/etc/nginx/conf.d$ sudo vim hexo_nginx.conf 
ubuntu@VM-176-3-ubuntu:/etc/nginx/conf.d$ ls
hexo_nginx.conf  thinkjs_nginx.conf
ubuntu@VM-176-3-ubuntu:/etc/nginx/conf.d$ cat hexo_nginx.conf 
server {
    listen 80;
    server_name blog.fuyix.cn;
    set $node_port 4000;

    location / {
	    proxy_pass http://localhost:$node_port;
    }
}
ubuntu@VM-176-3-ubuntu:/etc/nginx/conf.d$ sudo nginx -s reload
ubuntu@VM-176-3-ubuntu:/etc/nginx/conf.d$ 
```

注意：这里面的两个域名

```
server_name think.fuyix.cn;
server_name blog.fuyix.cn;
```

需要到域名管理商那里进行解析的，我把它解析成二级域名了而已：

![blog/server_conf_dns.png](http://images.fuyix.cn/blog/server_conf_dns.png)

到这里，所有的配置基本就完成了，下面就是启动我们的应用

```
ubuntu@VM-176-3-ubuntu:~/project_test$ node www/production.js 
[2017-02-24 11:37:57] [PRELOAD] preload packages finished 69ms
[2017-02-24 11:37:57] [THINK] Server running at http://127.0.0.1:8360/
[2017-02-24 11:37:57] [THINK] ThinkJS Version: 2.2.17
[2017-02-24 11:37:57] [THINK] Cluster Status: closed
[2017-02-24 11:37:57] [THINK] WebSocket Status: closed
[2017-02-24 11:37:57] [THINK] File Auto Compile: false
[2017-02-24 11:37:57] [THINK] File Auto Reload: false
[2017-02-24 11:37:57] [THINK] App Enviroment: production

ubuntu@VM-176-3-ubuntu:~/blog_server$ hexo server
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.

```

然后就是进行域名访问了：

![blog/server_conf_thinkjs.png](http://images.fuyix.cn/blog/server_conf_thinkjs.png)

![blog/server_conf_hexo.png](http://images.fuyix.cn/blog/server_conf_hexo.png)

