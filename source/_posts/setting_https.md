
---
title: 设置https
date: 2023-09-25 10:45:38
tags:
---
# Redirect HTTP to HTTPS
server {
    listen 80;
    listen [::]:80;
    server_name Your_Domain_Name;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name Your_Domain_Name;

location / {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Range $http_range;
    proxy_set_header If-Range $http_if_range;
    proxy_redirect off;
    proxy_pass http://127.0.0.1:8080;
# the max size of file to upload
    client_max_body_size 20000m;
}

}