---
title: KMS命令激活Windows/Office
date: 2023-09-26 17:42:03
tags:
- Windows
categories: 
- Windows
---
**注意**：以下所有命令均以管理员权限运行

**Windows激活**

1.安装秘钥  
`slmgr.vbs -ipk xxxx-xxxx-xxxx-xxxx`

2.设置KMS服务器  
`slmgr.vbs -skms xxx.xxx.xxx`

3.激活Windows  
`slmgr.vbs -ato`

4.查看激活信息  
`slmgr.vbs -xpr`

**Office激活**(VL版本)

**1.CMD切换到Office安装位置**  
默认安装位置 C:\\Program Files\\Microsoft Office\\Office14  
office16是office2016  
office15是office2013  
office14是office2010  
`cd "C:\Program Files\Microsoft Office\Office14"`

**2.设置KMS服务器**  
`cscript ospp.vbs /sethst:xxx.xxx.xxx`

**3.安装Office秘钥**  
`cscript ospp.vbs /inpkey:xxxxx`

**4.激活Office**  
`cscript ospp.vbs /act`

**可能会用到的ospp.vbs其它命令**

**1.卸载秘钥**(xxxxx为秘钥后5位)  
`cscript ospp.vbs /unpkey:xxxxx`  
**2.显示许可证信息**  
`cscript ospp.vbs /dstatus`