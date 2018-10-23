---
title: apache配置
key: 20160303
tags: apache
comment: true
---

## 修改 apache 服务器端口

Apache 这个web服务器的默认端口是在 80 端口进行监听，如果你访问一个网站，则默认端口就是 80

端口：一台电脑 可以有 65535 个端口，从1 到 65535，在应用中 可以通过 命令 `netstat -an` 来查看有哪些端口在监听

如下图所示：

![](http://images.fuyix.cn/17-5-15/32080090-file_1494858181799_179c3.png)

那么为什么要查看有哪些端口在监听呢？

作为一个服务器管理人员，当发现有些端口不应该被监听的时候，就应该将其杀死。一般来讲，端口号比较大的就比较危险，理论上讲，端口越少越安全，不要开的端口就尽量不要开，因为你这个端口打开后，别人就可以通过扫描你的端口，可能会对你进行攻击。

使用 `netstat -an` 命令，可以查看应用程序在使用那个端口，如下图所示，从而可以将不需要的关闭掉。

![](http://images.fuyix.cn/17-5-15/32335164-file_1494858437982_709.png)


一台机器的80端口被监听了，则该端口就不能再被其他的端口监听了，通常每个端口只能被使用一次。

端口分为有名端口 1-1024 号（这些端口自己不要使用），其他端口可以自己分配


Apache如何去配置端口？

Apache软件 配置是在 httpd.conf 文件中配置，该文件在Apache目录下的conf目录下

![](http://images.fuyix.cn/17-5-15/98273627-file_1494858553186_412.png)

用记事本打开 `httpd.conf` 文件，内容很多，但是，并不需要我们都看完

在 第60行，可以修改 Listen 端口，如下图所示：

![](http://images.fuyix.cn/17-5-15/73113995-file_1494858618023_12a41.png)

修改端口后，一定要重新启动Apache服务器才能生效。

## Apache 目录结构 

![](http://images.fuyix.cn/17-5-15/52674078-file_1494858701729_7fa1.png)

htdocs 是最重要的目录，是我们一直要使用的

htdocs 目录中存放了我们的网页文件，默认情况下打开该目录下的 index.html ，可以修改默认打开的网页，后面详细介绍如何修改

modules 目录中存放了Apache服务器所需要使用的模块，这些在httpd.conf 配置文件中进行了加载

![](http://images.fuyix.cn/17-5-15/60840528-file_1494858758980_7957.png)

Apache 加载了某个模块就会实现某些功能，在网上可以搜索 Apache HTTP 服务器 2.4 文档 ，在这个文档里对Apache进行了详细的介绍，包括各个模块的功能


## 虚拟目录

提一个需求
我的Apache安装在D盘，但是出现 D 盘空间不足的情况，E盘有更多的空间，能不能把 E盘的一个文件夹下的 网页文件当作网站管理

下面看看如何实现这个功能

首先我在E盘创建了一个文件夹 MyWeb ，MyWeb下有一个网页文件 test.html，如下图所示：

![](http://images.fuyix.cn/17-5-15/41088342-file_1494858836331_ee.png)

网上查了很多资料，也有很多方法，我用的是Apache2.4版本的，发现网上的方法都不能用，可能他们适用的是Apache2.2，不过我没有试过。那么既然不能用，就只有自己尝试来创建虚拟目录了。

查看网上的方法，发现在Apache2.4里产生错误的原因应该主要是设置文件夹的访问权限造成的。
网上设置文件夹的访问权限一般为如下代码：

```
<Directory "E:/MyWeb">
Order allow,deny
Allow from all
</Directory>
```

但是这样设置了之后，Apache就不能启动了，我想可能是因为Apache2.4的设置权限的命令修改了，导致原来支持2.2版本的命令在2.4版本中不能用了，那么这怎么办呢？

其实很好办，Apache2.4 默认的存放网页文件的目录是 Apache下的 htdocs这个目录，那么我们只要在 httpd.conf 文件中找到 htdocs目录的访问权限设置方法，对应着就可以知道 其他的目录权限怎么设置了。

很幸运，在 httpd.conf 中很容易找到这一段，如下图所示：

![](http://images.fuyix.cn/17-5-15/2094079-file_1494858908968_11213.png)

于是，根据这一段内容，我们来设置 MyWeb的访问权限，如下图所示：

![](http://images.fuyix.cn/17-5-15/88640369-file_1494858965571_a355.png)

这样就可以了，然后我们再来添加 虚拟目录，其实虚拟目录说到底就是一个网站别名，我可以使用一个自定义的名字来指定一个路径，这个路径指向一个目录，然后我在浏览器中输入这个别名就可以访问其指定的目录下的网页文件了，说起来就是这样的

在 httpd.conf 中，提供了 设置虚拟目录的方法，如下图所示：

![](http://images.fuyix.cn/17-5-15/51673183-file_1494859001810_c77.png)

我一般不想像上图中的方法在其后面直接添加，我喜欢像下面这种方法，自己另外添加一个alias_module 这样的节点，然后在这里面添加上自己想要设置的虚拟目录即可，这样可以不去破坏原有的文件，自己添加，好看也好查找

![](http://images.fuyix.cn/17-5-15/49726485-file_1494858952264_11726.png)

注意，每个虚拟目录都需要设置权限的，上面我只设置了 “E:/MyWeb” 目录的权限，要使用其他虚拟目录的话都要设置权限，如果不设置权限，就会出现一下问题，如下图所示：

![](http://images.fuyix.cn/17-5-15/58132677-file_1494859057982_208f.png)


下面我们来对 MyWeb1 目录进行权限设置，并重启 Apache 服务器，然后再在浏览器中测试，如下：

![](http://images.fuyix.cn/17-5-15/82565909-file_1494859094586_15564.png)


![](http://images.fuyix.cn/17-5-15/30743497-file_1494859085658_1480b.png)


上面是一种方法，只需要两步，就可以设置好虚拟目录，真是在方便了


## 虚拟主机

我们在本机调试网站的时候，一般都是用本地ip（127.0.0.1 或 localhost）来访问，没有用到域名。那么我想用域名来访问我的网页文件可以吗？这个是可以的，但是得首先把apache配置成为基于ip地址的虚拟主机。

首先，我们要知道 以 127 开头的 ip 地址都是指向本机的，也就是说并不只有127.0.0.1 这一个IP地址指向本机，127.0.0.1 - 127.0.0.255 这些地址都是指向本机的，都是可以用的，这些地址被称为环回地址，只是127.0.0.1 这个地址我们用的多一点而已。这一点我们可以 ping 一下，来看看是不是，如下图所示：

![](http://images.fuyix.cn/17-5-15/94881027-file_1494859176528_17452.png)

![](http://images.fuyix.cn/17-5-15/86126538-file_1494859173629_f95e.png)

![](http://images.fuyix.cn/17-5-15/90316263-file_1494859180157_9394.png)

像上面这样，如果说127.0.0.1 - 127.0.0.255 这些地址都可以用，那么我们就有足够多的ip地址来设置虚拟主机了。

下面来为Apache2.4 设置虚拟主机。

如果要在Apache服务器中创建站点，需要启用 httpd-vhosts.conf 文件，并在该文件中添加 <VirtualHost *:80></VirtualHost > 这样一个标签

首先 在httpd.conf文件中找到 Include conf/extra/httpd-vhosts.conf 这一句话，然后将其前面的 # 号去掉，这样 Apache 服务器在运行的时候就会加载 httpd-vhosts.conf 这个文件，这个文件就是设置虚拟主机的配置文件，那么我们打开这个文件

![](http://images.fuyix.cn/17-5-15/73605582-file_1494859237733_67f4.png)

如上图所示，我将虚拟主机的IP地址设置成了127.0.0.2，虚拟主机的默认网页存放目录修改成了 E:/MyWeb，然后保存设置，重启Apache服务器。在浏览器中测试如下：

![](http://images.fuyix.cn/17-5-15/83703030-file_1494859272790_a4e6.png)

注意：DocumentRoot "E:/MyWeb" 这句话后面的 目录 "E:/MyWeb" ，是要设置访问权限的，因为在设置虚拟目录的时候我已经设置过了，这里就没有再设置了，如下图所示：

![](http://images.fuyix.cn/17-5-15/7406970-file_1494859313659_14483.png)

这样就设置好虚拟主机了

配置好的虚拟主机要想被外部访问，必须在 DNS 服务器或windows系统中进行注册

打开 C:\Windows\System32\drivers\etc 下的hosts文件，用记事本打开，在后面添加如下图所示：

![](http://images.fuyix.cn/17-5-15/10741114-file_1494859349010_628c.png)

添加之后保存，在浏览器中输入 www.fuyi.com:8088 就可以访问 E:/MyWeb 下的网页文件了














