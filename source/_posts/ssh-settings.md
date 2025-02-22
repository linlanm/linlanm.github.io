---
title: SSH免密登录配置
date: 2023-11-13 20:55:11
tags:
- Linux
- SSH
categories:
- Linux
- SSH
---

**1.生成密钥**
在终端执行下列命令`ssh-keygen -t rsa -C "备注信息"`，按提示进行操作

**2.将生成的公钥文件复制到服务器上**
```
cd .ssh
touch authorized_keys               ->如果ssh中存在此文件则省略此步骤
cat id_rsa.pub >> authorized_keys   -> 将id_rsa.pub的内容追加到authorized_keys
```

**3.修改配置文件**
```
vim /etc/ssh/sshd_config

RSAAuthentication yes
PubkeyAuthentication yes
# The default is to check both .ssh/authorized_keys and .ssh/authorized_keys2
# but this is overridden so installations will only check .ssh/authorized_keys
AuthorizedKeysFile      .ssh/authorized_keys
```
这里有一点很重要，在你配置密钥登录成功之前，千万不要太自信将PasswordAuthentication 设置no，否则你密钥登录不了，然后又禁止密码登录，就悲剧了。在密钥登录设置成功之后，可以将PasswordAuthentication 设置为no，禁用密码登录了，比较安全。

**4.登录测试**
完成上述步骤后，就能够登录到远程主机而不会被提示输入密码，测试口令如下：`ssh remote_username@server_ip_address`

[原文链接](https://www.cnblogs.com/niunafei/p/12635233.html)

