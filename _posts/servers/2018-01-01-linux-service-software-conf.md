---
title:  Linux 服务器常用软件安装及配置
key: key-20180101
tags: 服务器配置
comment: true
---

## 设置管理员密码

执行下面命令，输入密码即可

```sh
sudo passwd
```

## 安装 nginx

```sh
sudo apt install nginx
```

配置文件放在：/etc/nginx 目录下

## 安装 nvm&nodejs

```sh
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash # 如果输入 nvm 没反应，重启下 shell 或者激活下shell：source ~/.bashrc
nvm ls-remote # 查看远程 nodejs 的可安装版本
nvm install v7.5.0 # 安装对应版本的 nodejs
nvm alias default 7.5.0 # 设置默认版本
```

## 安装 git

```sh
sudo apt-get install git
```

## 生成 ssh 公钥

命令行输入

```sh
ssh-keygen -t rsa -C "username@example.com"  # ( 注册的邮箱)，接下来点击enter键即可（也可以输入密码）
```

## 在 Coding 或者 GitHub 添加公钥

进入 `/root/.ssh/` 目录，执行

```
cat id_rsa.pub
```

复制其中全部内容，添加到账户 ` SSH 公钥 ` 页面中，公钥名称可以随意起名字。完成后点击 `添加`，然后输入密码或动态码即可添加完成。

完成后在命令行测试，首次建立链接会要求信任主机。

```sh
root@VM-176-3-ubuntu:~/.ssh# ssh -T git@git.coding.net
The authenticity of host 'git.coding.net (14.215.101.70)' can't be established.
RSA key fingerprint is SHA256:jok3FH7q5LJ6qvE7iPNehBgXRw51ErE77S0Dn+Vg/Ik.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'git.coding.net,14.215.101.70' (RSA) to the list of known hosts.
Hello fuyi501! You've connected to Coding.net via SSH successfully!
```

## 安装 thinkjs 2.x 版本

[thinkjs官网](https://thinkjs.org/zh-cn/doc/2.2/index.html)

```sh
npm install thinkjs@2 -g --registry=https://registry.npm.taobao.org --verbose
thinkjs --version # 或 thinkjs -V
thinkjs new project_path # project_path为项目存放的目录
cd project_path
npm install
npm start
```

## 使用 PM2 管理 thinkjs 服务

PM2 是一款专业管理 Node.js 服务的模块，非常建议在线上使用。使用 PM2 需要以全局的方式安装，如：`sudo npm install -g pm2`。安装完成后，命令行下会有 pm2 命令。

创建项目时，会在项目目录下创建名为 pm2.json 的配置文件，内容类似如下：

```json
{
  "apps": [{
    "name": "project_test", // 项目名称
    "script": "www/production.js",
    "cwd": "/home/project_test", // 项目实际路径
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

```sh
pm2 startOrReload pm2.json
```

## 安装 mysql 数据库

安装的时候记住输入的 mysql 密码，登录远程链接的时候都要用到，密码强度看情况而定。

```sh
sudo apt-get install mysql-server mysql-client 
sudo netstat -tap | grep mysql ## 查看 mysql 服务
```

### 设置 mysql 远程访问

使用 ` mysql -u root -p ` 登录 mysql 数据库后，输入 ` GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION; ` 和 ` FLUSH PRIVILEGES; ` 即可。

其中 ` root ` 为数据库登录用户名，` 123456 ` 为密码，这里要修改成你设置的密码。完整显示如下所示：

```sh
mysql -u root -p

mysql> GRANT ALL PRIVILEGES ON *.* TO 'ubuntu'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)

mysql> exit;
```

### 修改配置文件

将 mysql 配置文件 ` /etc/mysql/mysql.conf.d/mysqld.cnf `中的 bind_ip 注释掉保存重启服务即可。

```sh
ubuntu@VM-176-3-ubuntu:~$ sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf 
ubuntu@VM-176-3-ubuntu:~$ service mysql restart # 重启服务
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to restart 'mysql.service'.
Authenticating as: ubuntu,,, (ubuntu)
Password: 
==== AUTHENTICATION COMPLETE ===
ubuntu@VM-176-3-ubuntu:~$ 
```

## 安装 Mongodb 数据库

```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4

echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/testing multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list

sudo apt-get update

sudo apt-get install -y mongodb-org

sudo service mongod start
sudo service mongod stop
sudo service mongod restart

sudo apt-get purge mongodb-org*
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb
```

### 设置远程访问

` vim /etc/mongod.conf ` 修改 ` bindIP ` 为 ` 0.0.0.0 `

重启服务 `sudo service mongod restart`

## 注意

涉及到端口的使用，要在远程服务器（阿里云或腾讯云等）的安全组中开放 <strong>端口</strong>，才可以使用。
