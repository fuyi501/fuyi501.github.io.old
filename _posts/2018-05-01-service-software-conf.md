---
title:  服务器常用软件安装及配置
key: sum-20180103
tags: 服务器配置
comment: true
---

## 安装 nodejs

安装 nvm ：`curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash`

如果输入 nvm 没反应，则重启 shell 就好了

`nvm ls-remote` 查看远程 nodejs 的可安装版本

`nvm install` 安装nodejs

`nvm alias default` 设置默认版本

## 安装 thinkjs

`npm install thinkjs@2 -g --registry=https://registry.npm.taobao.org --verbose`

## 安装 nginx
`sudo apt install nginx`

配置文件放在：/etc/nginx 目录下

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
`vim /etc/mongod.conf` 修改 bindIP 为 0.0.0.0
重启服务 `sudo service mongod restart`

注意：要在服务器的安全组中开放 端口 `27017`
