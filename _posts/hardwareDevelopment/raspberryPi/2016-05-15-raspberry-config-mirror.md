---
title: 树莓派配置镜像
key: 20160515
tags: 树莓派
comment: true
---

科大开源镜像站提供了raspbian的软件包镜像，国内的用户可以选择改用科大镜像站作为更新源。

## 具体做法

修改之前，最好先备份原始的配置文件。例如，使用如下命令将两个源配置文件拷贝到 HOME 目录。

```sh
cp /etc/apt/sources.list /etc/apt/sources.list.backup
cp /etc/apt/sources.d/raspi.list /etc/apt/sources.d/raspi.list.backup
```

或者直接在原来配置文件的基础上修改，将原来的配置使用#注释掉。

## 修改 source.list

更新后的 `/etc/apt/sources.list` 如下:

```sh
# deb http://mirrordirector.raspbian.org/raspbian/ jessie main contrib non-free rpi
# Uncomment line below then 'apt-get update' to enable 'apt-get source'
# deb-src http://archive.raspbian.org/raspbian/ jessie main contrib non-free rpi

# use ustc mirror:
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main contrib non-free rpi
```


## 修改 raspi.list

更新后的`/etc/apt/sources.d/raspi.list`如下:

```sh
# deb http://archive.raspberrypi.org/debian/ jessie main ui
# Uncomment line below then 'apt-get update' to enable 'apt-get source'
# deb-src http://archive.raspberrypi.org/debian/ jessie main ui

# use ustc mirror:
deb http://mirrors.ustc.edu.cn/archive.raspberrypi.org/debian/ jessie main ui
```
