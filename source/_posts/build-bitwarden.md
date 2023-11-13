---
title: 自建密码管理器Bitwarden
date: 2023-11-13 21:19:54
tags:
- Nginx
- 安全
categories:
- Nginx
---

**1.前期准备**
软件安装：docker Nginx

**2.docker镜像拉取**
```
docker pull vaultwarden/server
```
**3.启动docker镜像**
```
docker run -d --name vaultwarden -v /vw-data/:/data/ -p 8080:80 vaultwarden/server:latest
```
**4.配置nginx**(示例文件)
```
# The `upstream` directives ensure that you have a http/1.1 connection
# This enables the keepalive option and better performance
#
# Define the server IP and ports here.
upstream vaultwarden-default {
  zone vaultwarden-default 64k;
  server 127.0.0.1:8080;
  keepalive 2;
}

# Needed to support websocket connections
# See: https://nginx.org/en/docs/http/websocket.html
# Instead of "close" as stated in the above link we send an empty value.
# Else all keepalive connections will not work.
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      "";
}

# Redirect HTTP to HTTPS
server {
    listen 80;
    listen [::]:80;
    server_name vaultwarden.example.tld;

    if ($host = vaultwarden.example.tld) {
        return 301 https://$host$request_uri;
    }
    return 404;
}

server {
    # For older versions of nginx appened http2 to the listen line after ssl and remove `http2 on`
    listen 443 ssl;
    listen [::]:443 ssl;
    http2 on;
    server_name vaultwarden.example.tld;

    # Specify SSL Config when needed
    #ssl_certificate /path/to/certificate/letsencrypt/live/vaultwarden.example.tld/fullchain.pem;
    #ssl_certificate_key /path/to/certificate/letsencrypt/live/vaultwarden.example.tld/privkey.pem;
    #ssl_trusted_certificate /path/to/certificate/letsencrypt/live/vaultwarden.example.tld/fullchain.pem;

    client_max_body_size 525M;

    location / {
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $connection_upgrade;

      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;

      proxy_pass http://vaultwarden-default;
    }

    # Optionally add extra authentication besides the ADMIN_TOKEN
    # Remove the comments below `#` and create the htpasswd_file to have it active
    #
    #location /admin {
    #  # See: https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/
    #  auth_basic "Private";
    #  auth_basic_user_file /path/to/htpasswd_file;
    #
    #  proxy_http_version 1.1;
    #  proxy_set_header Upgrade $http_upgrade;
    #  proxy_set_header Connection $connection_upgrade;
    #
    #  proxy_set_header Host $host;
    #  proxy_set_header X-Real-IP $remote_addr;
    #  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    #  proxy_set_header X-Forwarded-Proto $scheme;
    #
    #  proxy_pass http://vaultwarden-default;
    #}
}
```
**5.测试登录**
使用客户端或网页端测试功能是否正常

**6.禁止他人注册**
由于这个bitwarden服务器是供个人使用，我们在注册完账号后，要关闭注册功能，防止他人注册。
#### 先停止容器
```
docker stop vaultwarden
```
#### 设置环境变量不允许注册用户-e SIGNUPS_ALLOWED=false，再启动容器
```
docker run -d --name vaultwarden -v /vw-data/:/data/ -p 8080:80 -e SIGNUPS_ALLOWED=false vaultwarden/server:latest
```