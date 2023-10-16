---
title: Linux JDK环境配置
date: 2023-10-16 20:33:01
tags:
- Linux
categories:
- Linux
---

1.在/etc/profile文件最后添加以下内容

```
#set oracle jdk environment
export JAVA_HOME=/opt/jdk/jdk1.8.0_171/
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH 
``` 
JAVA_HOME设置为JDK解压目录
2.执行`source /etc/profile` 重新加载配置
