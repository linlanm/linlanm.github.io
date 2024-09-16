---
title: 更新vaultwarden服务端
date: 2024-09-16 13:47:49
tags:
- Nginx
categories:
- Nginx
---

更新非常简单，您只需确保保留了已挂载的卷。如果您使用（绑定挂载路径）的方式，则只需使用 `pull` 拉取最新版本的镜像，再使用 `stop` 和 `rm` 来停止和移除当前容器，然后与之前相同的方式启动一个新的容器即可：



```
# 拉取最新版本的镜像
docker pull vaultwarden/server:latest

# 停止并移除旧版本容器
docker stop vaultwarden
docker rm vaultwarden

# 使用已挂载的数据启动容器
docker run -d --name vaultwarden -v /vw-data/:/data/ -p 80:80 vaultwarden/server:latest
```

然后访问 [http://localhost:80](http://localhost/)
