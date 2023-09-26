---
title: Debian10缺少iwlwifi固件无法使用无线网络解决办法
date: 2023-09-26 17:49:32
tags:
- Linux
categories:
- Linux
---
**解决过程：**

1.使用有线网络连接到互联网;

2.编辑Debian10的软件源配置文件

`$ sudo vim /etc/apt/sources.list`

将`non-free`添加到源的后面

3.更新源

`$ sudo apt update`

4.安装firmware-iwlwifi包

`$ sudo apt install firmware-iwlwifi`

5.加载模块

`$ sudo modprobe -r iwlwifi`

`$ sudo modprobe iwlwifi`

**说明：**Debian是一个开源操作系统。因此，在安装Debian的时候，默认只安装自由软件，而非自由软件（non-free）则不会被默认安装。