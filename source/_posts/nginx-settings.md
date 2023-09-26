---
title: nginx反向代理及设置访问站点密码
date: 2023-09-26 17:50:48
tags:
---
1.在/etc/nginx/conf.d目录下修改站点配置文件,修改完成后重启服务

```
server {
        listen 80;
        server_name admin.service.com;
        location / {
               proxy_pass http://192.168.1.123:8090;
               index index.jsp index.html index.htm;
        }
    }

```
2.安装访问验证工具

Ubuntu: `# apt install apache2-utils`

CentOS: `# yum install httpd-tools`

3.设置验证用户名及密码

`# mkdir -p /usr/local/src/nginx/`

`# htpasswd -c /usr/local/src/nginx/passwd admis`  
passwd为密码文件，admis为用户名

4.修改nginx站点配置文件,修改完成后重启nginx服务

```
server {  
     listen 80;  
     server_name  localhost;  
     …….  
     #新增下面两行  
     auth_basic "Please input password"; #这里是验证时的提示信息  
     auth_basic_user_file /usr/local/src/nginx/passwd;  
     location /{  
     …….  
 }
 }
```