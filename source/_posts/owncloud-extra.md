---
title: owncloud安装后常见配置问题
date: 2023-09-26 17:47:03
tags:
- Nginx
- Linux
categories:
- Hexo
---
1.PHP 的设置似乎有问题, 无法获取系统环境变量. 使用 getenv(\\”PATH\\”) 测试时仅返回空结果。

编辑`/etc/php/7.3/fpm/php-fpm.conf`配置文件

```
env[PATH] = /usr/local/bin:/usr/bin:/bin:/usr/local/php/bin
```

将上面的代码粘贴到配置文件的最后一行，重启php7.3-fpm

2.HTTP 严格传输安全（Strict-Transport-Security）报头未配置到至少“15552000”秒。处于增强安全性考虑，我们推荐按照安全提示启用 HSTS。

编辑网站的配置文件“vhost.conf”文件找到对应网站的433端口段添加下面代码（保存后重启nginx）

```
add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
```