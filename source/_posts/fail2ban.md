---
title: 使用Fail2Ban动态封禁IP
date: 2025-02-21 22:26:24
tags:
- Linux
- Nginx
categories:
- Linux
- Nginx
---

`Fail2Ban` 可以动态封禁多次尝试登录失败的 IP。

### 步骤：

1.  **安装 `Fail2Ban`**：
    

```   
sudo apt install fail2ban
```

    
2.  **配置 `Fail2Ban`**：  
编辑配置文件 `/etc/fail2ban/jail.local`：
    
```
sudo vim /etc/fail2ban/jail.local
```
    
添加以下内容：
```    
[sshd]
enabled = true
maxretry = 3
bantime = 3600
``` 
3.  **重启 `Fail2Ban`**：  
重启 `Fail2Ban` 以应用配置：   
```  
sudo systemctl restart fail2ban
```    
4.  **验证配置**：  
查看封禁的 IP：
```  
sudo fail2ban-client status sshd
```    


