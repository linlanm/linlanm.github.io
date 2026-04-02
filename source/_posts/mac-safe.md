---
title: 无法打开“xxx”，因为无法确认开发者的身份提示解决
date: 2025-05-13 15:30:24
tags:
---

无法打开“xxx”，因为无法确认开发者的身份

无法打开“xxx”，因为它来自身份不明的开发者。

![无法打开“xxx”，因为无法确认开发者的身份。提示解决](https://img.macoshome.com/2023/04/wufaquerenkaifazhedeshenfenVenture0.jpg)

## 解决办法：

**第一种方法：**

**只要进入应用程序文件夹，对着软件图标 鼠标右键 – “打开” -“打开”即可。**

**第二种方法：**

1.**打开“任何来源”选项**

打开”系统偏好设置” – “安全性与隐私” – “允许从以下位置下载的App：”，没有看到“任何来源”选项被勾选的话，我们要用命令启用。

![无法打开“xxx”，因为无法确认开发者的身份。提示解决](https://img.macoshome.com/2023/04/venturasudospctlmaster-disable1.jpg)

我们打开启动台–其他–终端，打开终端；

![无法打开“xxx”，因为无法确认开发者的身份。提示解决](https://img.macoshome.com/2022/04/dakaizhognduan.png)

然后拷贝下面的命令并粘贴到终端工具里面按回车键运行；

sudo spctl --master-disable

运行之后会提示输入密码(下图所示)，然后输入你的Mac开机密码按回车键即可。

注意！密码是看不见的，输入完密码按回车键即可！

![无法打开“xxx”，因为无法确认开发者的身份。提示解决](https://img.macoshome.com/2021/05/sudospctl2.png)

最后我们再打开 “系统偏好设置” – “安全性与隐私” – “允许从以下位置下载的App：”，可以看到勾选已经变成了“任何来源”。

![无法打开“xxx”，因为无法确认开发者的身份。提示解决](https://img.macoshome.com/2023/04/venturasudospctlmaster-disable2.jpg)

然后我们再去打开一下APP，一会它还会提示“无法打开“xxx”，因为无法确认开发者的身份。”，我们前往”系统偏好设置” – “安全性与隐私” 面板，可以看到一个提示 “已阻止使用 Gas Station Simulators，因为无法确认开发者的身份。”，我们点击仍要打开即可就可以正常运行APP啦。

![无法打开“xxx”，因为无法确认开发者的身份。提示解决](https://img.macoshome.com/2023/04/wufaquerenkaifazhedeshenfenVenture2.jpg)

**2.你还可以使用命令来绕过应用签名认证解决 “无法打开“xxx”，因为无法确认开发者的身份。”**

绕过APP应用签名认证（即公正Gatekeeper）

> 使用到命令模式手动签名认证程序：
> 
> sudo xattr -rd com.apple.quarantine /Applications/LockedApp.app
> 
> /Applications/LockedApp.app就是要手动认证的APP。

我们打开启动台–其他–终端，打开终端，然后拷贝下面的命令并粘贴到终端工具里面；

sudo xattr -rd com.apple.quarantine

注意：sudo xattr -rd com.apple.quarantine后面需要加个空格，操作好后，先不要运行，终端不要关闭。

我们这里以Gas Station Simulator为例子，打开“访达” – “应用程序”文件夹找到要操作的APP，找到Gas Station Simulator的应用图标“GSS2”并把它拖到终端里面（如下图）：

![无法打开“xxx”，因为无法确认开发者的身份。提示解决](https://img.macoshome.com/2023/04/wufaquerenkaifazhedeshenfenVenture3.jpg)

拖好应用程序到终端之后按回车键运行，之后会提示输入密码(下图所示)，然后输入你的Mac系统开机密码，按回车键运行，注意！密码是看不见的；

![无法打开“xxx”，因为无法确认开发者的身份。提示解决](https://img.macoshome.com/2023/04/wufaquerenkaifazhedeshenfenVenture4.png)

运行成功之后，也不会再提示”无法打开“xxx”，因为无法确认开发者的身份“了。