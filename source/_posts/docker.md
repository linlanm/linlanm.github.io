---
title: Docker安装及配置
date: 2023-11-16 16:08:39
tags:
- 容器
categories:
- 容器
---

Docker 是一个开源的容器化平台，它允许你构建，测试，并且作为可移动的容器去部署应用，这些容器可以在任何地方运行。一个容器表示一个应用的运行环境，并且包含软件运行所需要的所有依赖软件。
Docker 是现代软件开发，持续集成，持续交付的一部分。
这篇教程将会涉及如何在 Ubuntu 上安装 Docker。
Docker 在标准的 Ubuntu 20.04 软件源中可用，但是可能不是最新的版本。这将会从 Docker 的官方软件源中安装最新的 Docker 软件包。

## 一、在 Ubuntu 20.04 上安装 Docker

在 Ubuntu 上安装 Docker 非常直接。通过启用 Docker 软件源，导入 GPG key，并且安装软件包。
首先，更新软件包索引，并且安装必要的依赖软件，来添加一个新的 HTTPS 软件源：

```text
sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

使用下面的 `curl` 导入源仓库的 GPG key：

```text
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

将 Docker APT 软件源添加到你的系统：

```text
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

现在，Docker 软件源被启用了，你可以安装软件源中任何可用的 Docker 版本。

01.想要安装 Docker 最新版本，运行下面的命令。如果你想安装指定版本，跳过这个步骤，并且跳到下一步。

```text
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```

02.想要安装指定版本，首先列出 Docker 软件源中所有可用的版本：

```text
sudo apt update
apt list -a docker-ce
```

可用的 Docker 版本将会在第二列显示。在写作这篇文章的时候，在官方 Docker 软件源中只有一个 Docker 版本（`5:19.03.9~3-0~ubuntu-focal`）可用：

```text
docker-ce/focal 5:19.03.9~3-0~ubuntu-focal amd64
```

通过在软件包名后面添加版本`=<VERSION>`来安装指定版本：

```text
sudo apt install docker-ce=<VERSION> docker-ce-cli=<VERSION> containerd.io
```

一旦安装完成，Docker 服务将会自动启动。你可以输入下面的命令，验证它：

```text
sudo systemctl status docker
```

输出将会类似下面这样：

```text
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2020-05-21 14:47:34 UTC; 42s ago
...
```

当一个新的 Docker 发布时，你可以使用标准的`sudo apt update && sudo apt upgrade`流程来升级 Docker 软件包。

如果你想阻止 Docker 自动更新，锁住它的版本：

```text
sudo apt-mark hold docker-ce
```

## 二、配置Docker镜像
国内从 DockerHub 拉取镜像有时会遇到困难，此时可以配置镜像加速器。Docker 官方和国内很多云服务商都提供了国内加速器服务，例如：

*   科大镜像：https://docker.mirrors.ustc.edu.cn
*   网易：https://hub-mirror.c.163.com
*   阿里云：https://<你的ID>.mirror.aliyuncs.com
*   七牛云加速器：https://reg-mirror.qiniu.com

1.新建或编辑daemon.json
```
vim /etc/docker/daemon.json
```
2.daemon.json中编辑如下
```
{
    "registry-mirrors": [
        "http://hub-mirror.c.163.com",
        "https://docker.mirrors.ustc.edu.cn",
        "https://registry.docker-cn.com"
    ]
}
```
3.重启docker
```
systemctl restart docker
```
4.执行`docker info`查看是否修改成功

当配置某一个加速器地址之后，若发现拉取不到镜像，请切换到另一个加速器地址。国内各大云服务商均提供了 Docker 镜像加速服务，建议根据运行 Docker 的云平台选择对应的镜像加速服务。
阿里云镜像[获取地址](cr.console.aliyun.com)，登陆后，左侧菜单选择“镜像加速器”就可以看到你的专属地址了。之前还有Docker官方加速器`https://registry.docker-cn.com`，现在好像已经不能使用了，可以多添加几个国内的镜像，如果有不能使用的，会切换到可以使用的镜像来拉取。

原文链接 1.[Docker安装](https://zhuanlan.zhihu.com/p/143156163?utm_id=0) 2.[Docker镜像配置](https://www.runoob.com/docker/docker-mirror-acceleration.html)3.[Docker镜像配置2](https://developer.aliyun.com/article/1294592)