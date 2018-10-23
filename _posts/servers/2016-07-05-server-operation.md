---
title: 服务器操作配置
key: 20160705
tags: 服务器
comment: true
---

## 设置管理员密码

执行下面命令，输入密码即可

```bash
sudo passwd
```

## 安装 nginx

```
sudo apt-get update
sudo apt-get install nginx
```

## 安装 nvm&node

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash # 重启后才有 nvm 命令
nvm ls-remote
nvm install v7.5.0
nvm alias default 7.5.0
```

## 安装 mysql
```
sudo apt-get install mysql-server mysql-client
sudo netstat -tap | grep mysql
```

```mysql
mysql -u root -p
mysql> 
mysql> GRANT ALL PRIVILEGES ON *.* TO 'ubuntu'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> 
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)

mysql> exit;
```

```
ubuntu@VM-176-3-ubuntu:~$ sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf 
ubuntu@VM-176-3-ubuntu:~$ service mysql restart
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to restart 'mysql.service'.
Authenticating as: ubuntu,,, (ubuntu)
Password: 
==== AUTHENTICATION COMPLETE ===
ubuntu@VM-176-3-ubuntu:~$ 
```

## 安装 mongodb



## 安装 thinkjs

```
npm install thinkjs@2 -g --verbose
thinkjs --version # 或 thinkjs -V
thinkjs new project_path; #project_path为项目存放的目录
npm install
npm start
```

## 安装 hexo

```
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server
```

## 安装 git

```
sudo apt-get install git
```

## 生成 ssh 公钥

命令行输入

```
ssh-keygen -t rsa -C "username@example.com"  #( 注册的邮箱)，接下来点击enter键即可（也可以输入密码）
```

## 在 Coding.net 添加公钥

进入 `/root/.ssh/` 目录，执行
```
cat id_rsa.pub
```

复制其中全部内容，添加到账户“SSH 公钥”页面 中，公钥名称可以随意起名字。完成后点击“添加”，然后输入密码或动态码即可添加完成。

完成后在命令行测试，首次建立链接会要求信任主机。

```
root@VM-176-3-ubuntu:~/.ssh# ssh -T git@git.coding.net
The authenticity of host 'git.coding.net (14.215.101.70)' can't be established.
RSA key fingerprint is SHA256:jok3FH7q5LJ6qvE7iPNehBgXRw51ErE77S0Dn+Vg/Ik.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'git.coding.net,14.215.101.70' (RSA) to the list of known hosts.
Hello fuyi501! You've connected to Coding.net via SSH successfully!
root@VM-176-3-ubuntu:~/.ssh# 
```

## 使用 PM2 管理 thinkjs 服务

PM2 是一款专业管理 Node.js 服务的模块，非常建议在线上使用。使用 PM2 需要以全局的方式安装，如：`sudo npm install -g pm2`。安装完成后，命令行下会有 pm2 命令。

创建项目时，会在项目目录下创建名为 pm2.json 的配置文件，内容类似如下：

```
{
  "apps": [{
    "name": "project_test",
    "script": "www/production.js",
    "cwd": "/home/ubuntu/project_test",
    "exec_mode": "cluster",
    "instances": 0,
    "max_memory_restart": "1G",
    "autorestart": true,
    "node_args": [],
    "args": [],
    "env": {
      
    }
  }]
}
```

将 cwd 配置值改为线上真实的项目路径，然后在项目目录下使用下面的命令来启动/重启服务：
```
pm2 startOrReload pm2.json
```