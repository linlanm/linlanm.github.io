---
title: 在Ubuntu22.04上安装Certbot
date: 2024-01-29 22:47:14
tags:
- Linux
- Nginx
categories:
- Linux
- Nginx
---

在当今的数字时代，确保网站的安全性至关重要。网络安全的一个基本方面是使用SSL/TLS证书来加密您的网站与其访问者之间传输的数据。Certbot是一个免费的开源工具，简化了获取和更新SSL/TLS证书的过程。

## 在 Ubuntu 22.04 LTS Jammy Jellyfish 上安装 Certbot

### 第 1 步。
在终端中运行以下命令，确保所有系统软件包都是最新的。

```
sudo apt update
sudo apt upgrade
```

### 第 2 步。
在 Ubuntu 22.04 上安装 Certbot。

**使用 Snap 包安装 Certbot**

安装 Certbot Snap 软件包：

```
sudo snap install --classic certbot
```

创建指向 Certbot 命令的符号链接：

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

通过运行以下命令验证 Certbot 是否已正确安装：

```
certbot --version
```

Certbot现在可以使用了。您可以通过运行以下命令来获取证书：

如果你的Web服务器是Apache，请使用以下命令：

```
sudo certbot --apache
```

如果您使用 Nginx 作为您的 Web 服务器，请改用以下命令：

```
sudo certbot --nginx
```

### 第 3 步。
证书续订配置。

**设置自动续订**

SSL/TLS 证书的有效期有限。Certbot可以为您自动化续订过程。要设置自动续订，请添加每天运行两次的 cron 作业：

```
sudo crontab -e
```

将以下行添加到 crontab 文件中：


```
0 0 * * * /usr/bin/certbot renew --quiet
```

此作业将每天检查一次即将过期的证书，并在必要时续订证书。

您可以使用以下命令手动测试续订过程：

```
sudo certbot renew --dry-run
```

### 第 4 步。
验证 SSL/TLS 证书安装。

**网页浏览器测试**

通过Web浏览器并访问您的网页，您应该会在地址栏中看到一个锁图标，表示连接安全。