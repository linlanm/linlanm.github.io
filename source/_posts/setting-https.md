---
title: 使用Let’s Encrypt配置HTTPS
date: 2023-09-26 17:40:09
tags:
- Linux
- Nginx
categories:
- Linux
- Nginx
---
注意：教程使用Ubuntu18.04+Nginx

**一、安装Certbot**

```
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install certbot  python-certbot-nginx
```

如果在执行上述命令时提示命令不存在，可能是没有安装add-apt-repository，请按以下命令安装

```
$ sudo apt-get install python-software-properties software-properties-common 
```

**二、生成证书**

执行以下命令，然后按照提示一步一步操作

```
$ sudo certbot --nginx
```

**备注：**  
privkey.pem 这是私匙，对应Nginx的ssl\_certificate\_key选项。  
cert.pem 服务器证书，对应SSLCertificateFile选项。  
chain.pem 除服务器证书之外的所有证书，Nginx对应ssl\_trusted\_certificate选项。  
fullchain.pem 包括上面的服务器证书和其他证书，Nginx对应ssl\_certificate选项。

**三、更新证书**

Let’s Encrypt的证书权威且安全，就是有效期只有90天。过期前需要续时间。运行命令`sudo certbot renew`即可续时间，如果还没到过期时间，运行命令也不会有大碍。当然你可以使用命令测试`sudo certbot renew --dry-run`。可以根据需要自己写一个脚本或者cron定时更新证书。

**四、常见问题及解决方法**
**Q**:ubuntu certbot无法从“urllib3.contrib”导入名称“appengine”;
**A**:执行命令`pip3 uninstall urllib3`卸载urllib3即可。