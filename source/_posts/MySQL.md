---
title: 安装绿色版MySQL
date: 2025-04-25 14:44:34
tags:
- Windows
- Database
categories:
- Windows
- Database
---

## 背景信息
MySQL是一个关系型数据库管理系统，由瑞典 MySQL AB 公司开发，属于 Oracle 旗下产品。MySQL是最流行的关系型数据库管理系统之一，在 WEB 应用方面，MySQL是最好的RDBMS (Relational Database Management System，关系数据库管理系统)应用软件之一。
MySQL是一种关系型数据库管理系统，关系数据库将数据保存在不同的表中，而不是将所有数据放在一个大仓库内，这样就增加了速度并提高了灵活性。
MySQL所使用的 SQL 语言是用于访问数据库的最常用标准化语言。MySQL 软件采用了双授权政策，分为社区版和商业版，由于其体积小、速度快、总体拥有成本低，尤其是开放源码这一特点，一般中小型和大型网站的开发都选择 MySQL作为网站数据库。


**安装环境**：Windows 10 Pro

## 1.下载zip安装包
进入官网下载社区版本，下载地址https://dev.mysql.com/downloads/
![](https://pic.imgdd.cc/item/6801f00e218de299cab289e7.png)

选择系统和软件版本，点击需要的版本下载
![](https://pic.imgdd.cc/item/6801f00f218de299cab28a4e.png)

进入页面后可以不登录，点击底部“No thanks, just start my download.”即可开始下载。
![](https://pic.imgdd.cc/item/6801f00e218de299cab289dc.png)

## 2.安装
### 2.1 解压zip包到安装目录
我的解压在D:\Program Files\mysql-8.4.5-winx64
解压后的文件目录
![](https://pic.imgdd.cc/item/6801f00e218de299cab28995.png)

### 2.2 配置环境变量
将解压文件夹下的bin路径添加到Path变量值中
![](https://pic.imgdd.cc/item/6801f00e218de299cab28a26.png)

### 2.3 配置初始化的my.ini文件
解压后的目录没有my.ini文件，可以自行创建。在安装根目录下添加 my.ini（新建文本文件，将文件类型改为.ini），写入基本配置：
```
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=D:\Program Files\mysql-8.4.5-winx64   
# 设置mysql数据库的数据的存放目录
datadir=D:\Program Files\mysql-8.4.5-winx64\data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8mb4
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8mb4
```

**注意**：其中的data目录不需要创建，下一步初始化工作中会自动创建。

## 3.安装MySQL

在安装时，必须以管理员身份运行cmd，否则在安装时会报错，会导致安装失败的情况

### 3.1 初始化数据库

在cmd窗口执行命令（管理员权限）：

```mysqld --initialize --console```
![](https://pic.imgdd.cc/item/6801f384218de299cab2ed00.png)
执行完成后，会打印 root 用户的初始密码，比如：```ldCNA!pez6>(```

　　**注意**：执行输出结果里面有一段：```A temporary password is generated for root@localhost: ldCNA!pez6>(```其中root@localhost:后面的```ldCNA!pez6>(```就是初始密码（不含首位空格）。在没有更改密码前，需要记住这个密码，后续登录需要用到。要是你关快了，或者没记住，删掉初始化生成的 ```data``` 目录，再执行一遍初始化命令，又会重新生成的。当然，也可以使用安全工具，强制修改密码。

### 3.2 安装服务
在cmd窗口执行命令（管理员权限）：
```mysqld --install [服务名]```
后面的服务名可以不写，默认的名字为 mysql。当然，如果你的电脑上需要安装多个MySQL服务，就可以用不同的名字区分了，比如 mysql5 和 mysql8。
![](https://pic.imgdd.cc/item/6801f573218de299cab34e90.png)
### 3.3 启动服务
在cmd窗口执行命令（管理员权限）：
![](https://pic.imgdd.cc/item/6801f573218de299cab34e91.png)

### 3.4 其它常用命令

```
net start mysql             #启动MySQL服务
net stop mysql              #停止MySQL服务
sc delete [服务名]          #删除MySQL服务
```
## 4.更改密码

在cmd窗口执行命令：
```mysql -u root -p```
![](https://pic.imgdd.cc/item/6801f85c218de299cab3d105.png)
这时候会提示输入密码，记住了上面第3.1步安装时的密码，填入即可登录成功，进入MySQL命令模式。

在MySQL中执行命令：

```alter user 'root'@'localhost' identified by '新密码';  ```

修改密码，注意命令尾的；一定要有，这是MySQL的语法。
到此，安装部署就完成了。

## 5.其它常用命令
```
#授权所有权限 
GRANT ALL PRIVILEGES ON *.* TO 'xxh'@'%';
#授权基本的查询修改权限，按需求设置
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER ON *.* TO 'xxh'@'%';
#查看用户权限
show grants for 'xxh'@'%';
```

