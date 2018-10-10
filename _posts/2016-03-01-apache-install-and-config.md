---
title: apache安装和配置
key: 20160301
tags: apache
comment: true
---

## Apache下载：

![enter description here][1]

![enter description here][2]

![](http://images.fuyix.cn/17-5-15/13206631-file_1494861369623_130ff.png)

![](http://images.fuyix.cn/17-5-15/43432806-file_1494861349998_2d7e.png)

![](http://images.fuyix.cn/17-5-15/50921871-file_1494861356071_f1b4.png)


## Apache安装

Apache2.4的安装其实说来很简单，只需要修改httpd.conf 配置文件，然后使用命令行运行即可。

下载的http2.4安装包是一个压缩包形式，先将其解压到一个文件夹，我的解压目录是：D:\Program Files\Apache24，如下图所示：

![](http://images.fuyix.cn/17-5-15/7602337-file_1494861475766_f743.png)

打开conf目录，用记事本打开conf目录下的httpd.conf 配置文件，接下来就是修改这个文件

首先，修改第38行，如下图所示，将原来的路径修改成Apache的安装路径

![](http://images.fuyix.cn/17-5-15/96056252-file_1494861525025_15b89.png)

接下来修改监听端口，一般Apache会监听80端口，但是80端口可能会被其他的应用使用，所以这里一般要修改成其他的端口，如我的为8088，这样不会和其他应用冲突，端口设置这里自己修改的话改成1024-65536 之间的任一个值就可以，如下图所示：

![](http://images.fuyix.cn/17-5-15/38567354-file_1494861529735_4a4.png)

只需要修改这两个地方就可以了，然后在命令行运行

先打开 cmd ，也就是命令提示符，进入到Apache的bin目录下 D:\Program Files\Apache24\bin，如下图所示：

![](http://images.fuyix.cn/17-5-15/29543165-file_1494861570375_7c4a.png)

再输入命令 httpd ，就可以运行了

![](http://images.fuyix.cn/17-5-15/50867202-file_1494861596683_10089.png)

这个时候我们用浏览器来测试下，在浏览器中输入 localhost:8088，回车，出现如下图所示的信息就表示Apache服务器安装成功了

![](http://images.fuyix.cn/17-5-15/5406029-file_1494861653991_16631.png)

这个时候虽然Apache服务器已经安装成功并运行起来了，但是在windows的服务中并没有添加进来，也就是在windows的服务中找不到Apache这个服务，而且 ApacheMonitor 应用也是不可用的，如下图所示：

![](http://images.fuyix.cn/17-5-15/73941678-file_1494861703683_bac.png)

![](http://images.fuyix.cn/17-5-15/77708070-file_1494861698452_77d2.png)

这个时候我们需要安装Apache服务到系统服务里去，上面我们打开命令提示符没有使用管理员权限，如果这个时候输入命令 httpd -k install 的话，会出现如下错误：

![](http://images.fuyix.cn/17-5-15/51051535-file_1494861747570_b90b.png)

拒绝访问，是因为没有管理员的权限，所以要用管理员权限打开，打开之后，再进入到Apache服务器的bin目录下，然后输入命令  httpd -k install，如下图所示：

![](http://images.fuyix.cn/17-5-15/36038525-file_1494861604664_f6b3.png)

如果想卸载Apache服务，就输入命令：httpd -k uninstall ，这样就可以了

安装成功后，就可以在系统服务里看到Apache服务了，如下图所示：

![](http://images.fuyix.cn/17-5-15/1146330-file_1494861791294_152d6.png)

这时， ApacheMonitor 应用也可以使用了，如下图：

![](http://images.fuyix.cn/17-5-15/37728476-file_1494861811224_63b5.png)

启动成功后，如下图所示：

![](http://images.fuyix.cn/17-5-15/67428134-file_1494861828791_14a40.png)

这个时候，我们的Apache服务器就可以使用了

上面讲了两种方法来启动和停止Apache服务，其实我们也可以在命令行通过命令来启动或停止服务

命令：httpd -k start  ，用来启动Apache服务，命令：httpd -k stop，用来停止Apache服务

用命令行来启动Apache服务的话，如果我们每次都 cd 到Apache的安装目录下，就会显得很麻烦，这时可以添加环境变量，添加了环境变量后，我们就不用每次都 cd  到Apache的目录下了，添加环境变量的步骤如下：（我的是win10系统）

![](http://images.fuyix.cn/17-5-15/49259221-file_1494861852015_1ba9.png)

这个时候再打开命令提示符，直接输入 httpd -k start 就可以启动Apache服务了

![](http://images.fuyix.cn/17-5-15/96108921-file_1494861624676_3db5.png)


测试，同样成功

![](http://images.fuyix.cn/17-5-15/98354993-file_1494861909463_8b26.png)

到这里，对Apache服务器的安装说明就差不多了，没有过多的介绍httpd.conf 配置文件，如果有兴趣，可以看我写的下一篇文章。


  [1]: http://images.fuyix.cn/f9c02088-3560-4a2c-a078-2974124d2f74.png
  [2]: http://images.fuyix.cn/3a807f28-62aa-4237-86f7-d124212ef996.png