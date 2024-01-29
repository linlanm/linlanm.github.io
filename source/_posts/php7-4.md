---
title: Ubuntu2204安装PHP7.4
date: 2024-01-29 23:05:26
tags:
- Nginx
categories:
- Nginx
---

## 更新Ubuntu

首先，更新你的Ubuntu服务器：

```sql
sudo apt update && sudo apt upgrade
```

## 添加PHP存储库

要安装PHP7.4，需要使用第三方存储库。

首先，请确保已安装以下软件包，以便可以添加存储库：

```
sudo apt install software-properties-common
```

接下来，添加PHP存储库：

```
sudo add-apt-repository ppa:ondrej/php
```

最后，更新安装包：

```
sudo apt update
```

## 安装PHP 7.4

添加存储库后，可以使用以下命令安装PHP 7.4：

```
sudo apt install php7.4
```

此命令将安装其他软件包：

*   libapache2-mod-php7.4
*   libaprutil1-dbd-sqlite3
*   php7.4-cli
*   php7.4-common
*   php7.4-json
*   php7.4-opcache
*   php7.4-readline
*   等等……  
    就是这样。要检查服务器上是否已安装PHP 7.4，请运行以下命令：

```undefined
php -v
```

## 安装PHP7.4模块

根据你的应用程序，你可能需要其他软件包和模块。可以使用以下命令安装最常用的模块：

```
sudo apt install php-pear php7.4-curl php7.4-dev php7.4-gd php7.4-mbstring php7.4-zip php7.4-mysql php7.4-xml php7.4-fpm
```

就这样，你就可以在Ubuntu服务器上开始使用PHP。

## Nginx反代配置

在 /etc/nginx/conf.d/**.conf 中增加PHP相关的配置  

```
      server {
            listen       80;
            server_name  yourdomainname;
            root         /var/www/html;
            
          #  location / {
          #     root   /var/www/html;
          #     index  index.html index.htm;
          #  }

            location ~ \.php$ {
               include snippets/fastcgi-php.conf;
               fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
            }
        }
```

其中 unix:/var/run/php/php7.4-fpm.sock 这个参数在 /etc/php/7.4/fpm/pool.d/www.conf 文件中配置，如果想做负载均衡，也可以把监听端口配置成 IP:Port 的形式。

修改完成后，使用 nginx -t 命令，测试 nginx.conf 文件，如果正常，重启 nginx，生效。

