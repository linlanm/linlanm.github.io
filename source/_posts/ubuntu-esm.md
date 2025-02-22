---
title: Ubuntu启用ESM
date: 2025-02-22 16:38:06
tags:
- Linux
categories:
- Linux
---

2021年04月，Ubuntu 16.04 LTS（Xenial Xerus）停止为期5年的标准支持维护，Ubuntu 16.04 LTS后续将过渡到扩展安全维护（ESM）的阶段。您可以参考本文的操作，通过Ubuntu订阅服务（UA-I）为Ubuntu 16.04实例获取ESM更新支持。

## 背景信息

当Ubuntu 16.04 LTS处于扩展安全维护（ESM）阶段时，您可以通过Ubuntu订阅服务（UA-I）内的扩展安全维护（ESM），继续获取基础操作系统、关键软件包和基础设置组件的安全更新。更多信息，请参见[Ubuntu Advantage for Infrastructure](https://ubuntu.com/advantage)以及[Extended Security Maintenance](https://ubuntu.com/security/esm)。

如果您需要在Ubuntu 16.04实例中继续获取操作系统的安全更新，请参见以下操作步骤。

## 步骤一：在Ubuntu官网订阅UA-I服务

1.  在本地主机，访问[Ubuntu官网](https://ubuntu.com/)。
    
2.  在顶部菜单栏，单击**Sign in**。
    
    您需要使用Ubuntu账号登录，如果您没有Ubuntu账号，需要根据实际的页面提示完成注册并登录Ubuntu官网。
    
3.  访问[Subscription](https://ubuntu.com/pro/subscribe)页面，然后单击**Register**。
    
    
4.  在**Free Personal Token**区域，获取并自行保存Token值。
    

## 步骤二：在Ubuntu 16.04实例中添加UA-I并进行安全更新

1.  远程连接Ubuntu 16.04实例。
    
    具体操作，请参见[通过密码或密钥认证登录Linux实例](https://help.aliyun.com/zh/ecs/user-guide/connect-to-a-linux-instance-by-using-a-password-or-key)。
    
2.  依次运行以下命令，安装最新的Ubuntu Advantage（UA）客户端。
    
    1.  从软件仓库源中更新软件包信息。
        
        ```shell
        sudo apt update
        ```
        
    2.  安装UA客户端。
        
        ```shell
        sudo apt install ubuntu-advantage-tools
        ```
        
    
3.  运行**步骤一**中保存的Token值。
    
    命令格式如下所示，您需要将`<token>`替换为实际保存的Token值。
    
    ```shell
    sudo ua attach <token>
    ```
    
4.  （可选）运行以下命令，启用ESM。
    
    如果您不确定ESM是否启用，可以运行以下命令确保ESM已启用。
    
    ```shell
    sudo ua enable esm-infra
    ```
    
5.  依次运行以下命令，更新并升级软件包。
    
    1.  从软件仓库源中更新软件包信息。
        
        ```shell
        sudo apt update
        ```
        
    2.  升级软件包。
        
        ```shell
        sudo apt upgrade 
        ```

[原文链接](https://help.aliyun.com/zh/ecs/user-guide/security-updates-after-the-end-of-support-and-maintenance-for-ubuntu-16-04-lts)