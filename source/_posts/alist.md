---
title: 手动安装AList及配置反向代理
date: 2025-02-03 12:31:36
tags:
- Linux
- Nginx
categories:
- Linux
- Nginx
---


## **获取 AList**

打开 [AList](https://github.com/Xhofe/alist/releases) 下载待部署系统对应的文件。最新版的前端已经和后端打包好了，不用再下载前端文件了。

xxxx 指的是不同系统/架构对应的名称，一般 Linux-x86/64 为 alist-linux-amd64

手动安装如果有如下提示：是因为[你的 GLIBC 版本太低](https://alist.nn.ci/zh/faq/why.html#lib64-libc-so-6-version-glibc-2-28-not-found-required-by-alist-%E6%88%96%E8%80%85-accept-function-not-implemented)，建议下载 musl 版本

```
lib64/libc.so.6: version `GLIBC_2.28' not found (required by ./alist)  
#或者
accept: function not implemented
```

当你看到 `start server@0.0.0.0:5244` 的输出，之后没有报错，说明操作成功。 第一次运行时会输出初始密码。程序默认监听 5244 端口。 现在打开 `http://ip:5244` 可以看到登录页面，WebDAV 请参阅 [WebDav](https://alist.nn.ci/zh/guide/webdav.html)。

  

## **手动运行**

v3.25.0以上版本将密码改成加密方式存储的hash值，无法直接反算出密码，如果忘记了密码只能通过重新 **`随机生成`** 或者 **`手动设置`**

**以Linux系统为例**

```
# 解压下载的文件，得到可执行文件：
tar -zxvf alist-xxxx.tar.gz
# 授予程序执行权限：
chmod +x alist
# 运行程序
./alist server

# 获得管理员信息 以下两个不同版本，新版本也有随机生成和手动设置
# 低于v3.25.0版本
./alist admin

# 高于v3.25.0版本
# 随机生成一个密码
./alist admin random
# 手动设置一个密码 `NEW_PASSWORD`是指你需要设置的密码
./alist admin set NEW_PASSWORD
```

  

## **守护进程**

**以Linux系统为例**
使用任意方式编辑 `/usr/lib/systemd/system/alist.service` 并添加如下内容，其中 path\_alist 为 AList 所在的路径

```
[Unit]
Description=alist
After=network.target
 
[Service]
Type=simple
WorkingDirectory=path_alist
ExecStart=path_alist/alist server
Restart=on-failure
 
[Install]
WantedBy=multi-user.target
```

然后，执行 `systemctl daemon-reload` 重载配置，现在你可以使用这些命令来管理程序：

*   启动: `systemctl start alist`
*   关闭: `systemctl stop alist`
*   配置开机自启: `systemctl enable alist`
*   取消开机自启: `systemctl disable alist`
*   状态: `systemctl status alist`
*   重启: `systemctl restart alist`

守护进程不配置? [**视频教程**](https://www.bilibili.com/video/BV1rF41197Qv?t=187.0)

## **相关信息**

对于所有平台，您可以使用以下命令来静默启动、停止和重新启动。 （v3.4.0 及更高版本）

```
# 携带`--force-bin-dir`参数启动服务
alist start
# 通过pid停止服务
alist stop
# 通过pid重启服务
alist restart
```

  

## **如何更新**

下载新版Alist，把之前的替换了即可。

  

*   启动: `systemctl start alist`
*   关闭: `systemctl stop alist`
*   状态: `systemctl status alist`
*   重启: `systemctl restart alist`

## **获取密码**

需要进入脚本安装AList的目录文件夹內执行如下命令

#### 低于v3.25.0版本

```
./alist admin
```

#### 高于v3.25.0版本

3.25.0以上版本将密码改成加密方式存储的hash值，无法直接反算出密码，如果忘记了密码只能通过重新 **`随机生成`** 或者 **`手动设置`**

```
# 随机生成一个密码
./alist admin random
# 手动设置一个密码,`NEW_PASSWORD`是指你需要设置的密码
./alist admin set NEW_PASSWORD
```

## **一直在加载怎么办?**

挂载了一些网盘但是不能用了重启了一下AList，发现进不去 网页提示：`获取设置失败：请稍后，正在加载存储`怎么办？

1.  等待几分钟
2.  通过使用命令将`失效的/无法启动的`存储停止运行

**以Linux系统为例**
如果通过命令停止 必须先进入你AList所在的文件夹输入命令
如果我们不知道是那个存储原因导致的，可以通过命令列出所有的存储

```
./alist storage list
```

```
[root@OPSD-g8xXordx3B9f alist]# ./alist storage list
INFO[2023-11-23 17:54:10] reading config file: data/config.json
INFO[2023-11-23 17:54:10] load config from env with prefix: ALIST_
INFO[2023-11-23 17:54:10] init logrus...
INFO[2023-11-23 17:54:10] Found 2 storages
┌─────────────────────────────────────────────────────────────────┐
│ ID    Driver            Mount Path                      Enabled │
│─────────────────────────────────────────────────────────────────│
│ 1     S3                /R2                             true    │
│ 2     UrlTree           /233                            true    │
└─────────────────────────────────────────────────────────────────┘
```

输入查询命令后我们会进入另一种模式无法输入，如果添加的存储过多可以通过键盘的 ↑ 和 ↓ 来往下翻，等找到后可以按`Ctrl+C`退出

例如我们是因为 `233` 这个存储停止的，我们就输入命令来停止，然后在 重启一下AList就可以了

```
./alist storage disable /233
```

```
[root@OPSD-g8xXordx3B9f alist]# ./alist storage disable /233
INFO[2023-11-23 17:54:52] reading config file: data/config.json
INFO[2023-11-23 17:54:52] load config from env with prefix: ALIST_
INFO[2023-11-23 17:54:52] init logrus...
INFO[2023-11-23 17:54:52] Storage with mount path [/233] have been disabled
```

## **设置反向代理**

[Andy Hsu](https://i.nn.ci/)2022年9月11日GuideInstallGuide大约 2 分钟

程序默认监听 5244 端口。如有修改，请一并修改下列配置中的端口号。如果你使用反向代理，建议你设置[site\_url](https://alist.nn.ci/zh/config/configuration.html#site_url)，以帮助alist更好的工作。
## **nginx**

在网站配置文件的 server 字段中添加

```
location / {
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header X-Forwarded-Proto $scheme;
  proxy_set_header Host $http_host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header Range $http_range;
	proxy_set_header If-Range $http_if_range;
  proxy_redirect off;
  proxy_pass http://127.0.0.1:5244;
  # the max size of file to upload
  client_max_body_size 20000m;
}
```

如果需要使用HTTP/3，需要将对应`HOST`行修改为：

```
proxy_set_header Host $host:$server_port;
```

这样修改后的配置同时也可以兼容HTTP/2及更低版本的请求。

**注意**

如果使用宝塔面板，请务必删除以下默认配置

```
- location ~ ^/(\.user.ini|\.htaccess|\.git|\.svn|\.project|LICENSE|README.md
- location ~ .\*\.(gif|jpg|jpeg|png|bmp|swf)$
- location ~ .\*\.(js|css)?$
```

并在`/www/server/nginx/conf/proxy.conf`中或对应网站配置文件中设置禁用Nginx缓存，否则默认配置下访问较大文件时Nginx会先尝试将远程文件缓存至本机，导致播放失败

```
proxy_cache cache_one; # 删除这一行
proxy_max_temp_file_size 0; #加上这一行
```

## **Apache**

在 VirtualHost 字段下添加配置项 ProxyPass，如：

```
<VirtualHost *:80>
    ServerName myapp.example.com
    ServerAdmin webmaster@example.com
    DocumentRoot /www/myapp/public

    AllowEncodedSlashes NoDecode
    ProxyPreserveHost On
    ProxyPass "/" "http://127.0.0.1:5244/" nocanon
    ProxyPassReverse "/" "http://127.0.0.1:5244/" nocanon
</VirtualHost>
```

## **Caddy**

在 Caddyfile 文件下添加 reverse\_proxy，如：

```
:80 {
  reverse_proxy 127.0.0.1:5244
}
```

如果部署在 443 端口正常使用的服务器上且使用域名进行访问，建议使用这种配置让 Caddy 自动申请证书：

```
example.com {
  reverse_proxy 127.0.0.1:5244
}

将 `example.com` 替换为你自己解析后的域名。

```

[原文链接](https://alist.nn.ci/zh/guide/install/manual.html)
