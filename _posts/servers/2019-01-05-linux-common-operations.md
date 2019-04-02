---
title:  Linux 常用操作
key: key-2019-0105
tags: linux
comment: true
---

## Linux 用户管理

Linux系统是一个多用户多任务的分时操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。用户的账号一方面可以帮助系统管理员对使用系统的用户进行跟踪，并控制他们对系统资源的访问；另一方面也可以帮助用户组织文件，并为用户提供安全性保护。每个用户账号都拥有一个惟一的用户名和各自的口令。用户在登录时键入正确的用户名和口令后，就能够进入系统和自己的主目录。

用户账号的管理工作主要涉及到用户账号的添加、修改和删除。
```
# 添加用户
useradd -d 用户目录 -m 用户名 -s /bin/bash

# 删除用户
userdel -r 用户名

# 修改用户
usermod 选项 用户名

#设置密码
passwd 用户名
```
## tmux 使用

Tmux 可用于在一个终端窗口中运行多个终端会话。不仅如此，还可以通过 Tmux 使终端会话运行于后台或是按需接入、断开会话，这个功能非常实用。

1. Tmux的使用场景
- 可以某个程序在执行时一直是输出状态，需要结合nohup、&来放在后台执行，并且ctrl+c结束。这时可以打开一个Tmux窗口，在该窗口里执行这个程序，用来保证该程序一直在执行中，只要Tmux这个窗口不关闭
- 下班后，你需要断开ssh或关闭电脑，将运行的命令或任务放置后台运行。
- 关闭终端,再次打开时原终端里面的任务进程依然不会中断

2. tmux 安装

- ubuntu 
    ```
    sudo apt-get install tmux
    ```
- mac os
    ```
    安装 Homebrew
    $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    
    安装tmux
    $ brew install tmux
    ```

3. tmux 使用

名词解释：tmux 会话，一个会话可以包含多个窗口，一个窗口可以包含多个面板。

进入tmux面板后，一定要先按 ctrl+b，然后松开，再按其他的组合键才生效。
 
常用到的几个组合键：
```
# 系统操作
ctrl+b ?           显示快捷键帮助
ctrl+b d           脱离当前会话，返回Shell界面
ctrl+b s           以菜单方式显示和选择会话
ctrl+b $           重命名当前会话

# 窗口操作
ctrl+b c           创建新窗口
ctrl+b &           关闭当前窗口
ctrl+b w           以菜单方式显示及选择窗口
ctrl+b 数字键       切换至指定窗口
ctrl+b ,           重命名当前窗口
ctrl+b .           修改当前窗口编号；相当于窗口重新排序
ctrl+b f           在所有窗口中查找指定文本

# 面板操作
ctrl+b "           模向分隔窗格
ctrl+b %           纵向分隔窗格
ctrl+b x           关闭当前面板
ctrl+b !           把当前面板变为新的窗口
ctrl+b q           显示面板的编号，当数字出现的时候按数字几就选中第几个面板
ctrl+b 方向键      选择面板
ctrl+b z           最大化和最小化面板
ctrl+b [           复制模式，可以使用方向键或者Pgup、Pgdn查看历史记录，按 q 推出
ctrl+b ctrl+方向键    调整面板大小

ctrl+b t           显示时钟。然后按enter键后就会恢复到shell终端状态
```

常用的命令：
```
tmux ls
tmux new -s 会话名
tmux a -t 会话名
tmux kill-session -t 会话名
tmux rename -t oldname newname
```

## 查看端口占用情况
```
1. netstat -an 
2. netstat -an|grep ':80' ## grep (global search regular expression(RE) and print out the line,全面搜索正则表达式并把行打印出来)是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。
3. lsof -i:8080
```

## Nginx 服务器 403 访问不了

如果你的 nginx 服务器已经配置好，域名解析正确，安全组端口开放，但是通过域名访问服务器出现403 错误，其实已经说明访问到了数据，基本是nginx配置文件的问题，查看 nginx 日志，发现是权限问题，将 nginx.conf 配置文件开头的 user 用户设置成 root 就可以了。
