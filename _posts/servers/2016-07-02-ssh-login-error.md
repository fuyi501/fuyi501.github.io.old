---
title: ssh 登陆服务器错误
key: 20160702
tags: ssh 服务器
comment: true
---

SSH登陆错误 WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:J4UQ9WzxiNz4LnQLpoxaLw7AbSXvlPpWNKnDm5A604E.
Please contact your system administrator.
Add correct host key in /home/fuyi/.ssh/known_hosts to get rid of this message.
Offending RSA key in /home/fuyi/.ssh/known_hosts:1
  remove with:
  ssh-keygen -f "/home/fuyi/.ssh/known_hosts" -R 119.29.163.61
RSA host key for 119.29.163.61 has changed and you have requested strict checking.
Host key verification failed.

打开 .ssh 目录下的 known_hosts 文件，把里面与所要连接 IP(119.29.163.61) 相关的内容删掉即可.