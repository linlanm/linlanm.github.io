---
title: Manjaro安装后基本配置
date: 2023-09-26 17:52:50
tags:
- Linux
categories:
- Linux
---
**1.设置官方镜像源**  
`$ sudo pacman-mirrors -i -c China -m rank`  
输入以上命令后会有弹出框，选择一个国内镜像(推荐中国科技大学的镜像源https://mirrors.ustc.edu.cn)

**2.升级系统及编辑器**  
`$ sudo pacman -Syyu && sudo pacman -S vim`

**3.添加archlinuxcn源**  
`$ sudo vim /etc/pacman.conf`  
文件底部添加以下几行


```
[archlinuxcn]
SigLevel = Optional TrustedOnly  
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

修改文件后，执行以下命令  
`$ sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring`

**4.安装输入法**(基于 fcitx 的搜狗输入法)  
`$ sudo pacman -S fcitx-im fcitx-configtool fcitx-sogoupinyin`  
添加输入法配置文件  
`$ vim ~/.xprofile`

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

重启即可正常使用输入法

5.安装常用软件(按需安装)  
`$ sudo pacman -S deepin.com.qq.office deepin.com.qq.im netease-cloud-music wps-office ttf-wps-fonts yay google-chrome`  
`$ yay -S deepin-wine-wechat`

deepin.com.qq.office \[QQ\]  
deepin.com.qq.im \[TIM\]  
netease-cloud-music \[网易云音乐\]  
wps-office \[WPS\]  
ttf-wps-fonts \[WPS字体\]  
yay \[AUR工具\]  
google-chrome \[谷歌浏览器\]  
deepin-wine-wechat \[微信\]