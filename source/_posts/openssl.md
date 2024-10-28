---
title: mac Monterey手动编译openssl
date: 2024-10-28 20:33:41
tags:
- macOS
categories:
- macOS
---
## 前言

mac Monterey 通过homebrew 安装 openssl 失败，被嫌弃系统太老，make test 测试不通过


## 下载openssl，并解压

注意：brew安装软件时，看他下载的是什么版本，就手动下载什么版本

下载地址:

```
https://www.openssl.org/source/openssl-3.4.0.tar.gz
```

## 配置openssl

进入解压的目录，执行命令

```shell
perl ./Configure --prefix=/usr/local/Cellar/openssl@3/3.4.0 --openssldir=/usr/local/openssl@3 --libdir=lib no-ssl3 no-ssl3-method no-zlib darwin64-x86_64-cc enable-ec_nistp_64_gcc_128
```


## 编译安装

### 1\. 编译

```shell
make
```

### 2\. 测试【可选】

```shell
make test
```

### 3\. 安装

```shell
sudo make install MANDIR=/usr/local/Cellar/openssl@3/3.4.0/share/man MANSUFFIX=ssl
```

查看是否安装完成，新增一个终端，输入以下命令

```shell
openssl version
```


```shell
which -a openssl
```

### 4\. brew链接openssl

使用brew链接openssl后，可使用brew管理手动安装的openssl

```shell
brew link openssl@3
```
