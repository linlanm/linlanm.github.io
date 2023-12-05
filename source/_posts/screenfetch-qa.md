---
title: screenfetch报错"/usr/bin/screenfetch行1851"解决方法
date: 2023-11-17 15:10:34
tags:
- Linux
categories:
- Linux
---

在debian12（testing）上运行screenfetch报错，如下所示：

```text
~$ screenfetch
/usr/bin/screenfetch: 行 1851: -: 语法错误：需要操作数（错误记号是 "-"）
         _,met$$$$$gg.           ×××××@deb-test
      ,g$$$$$$$$$$$$$$$P.        OS: Debian 12 bookworm
    ,g$$P""       """Y$$.".      Kernel: x86_64 Linux 6.1.0-7-amd64
   ,$$P'              `$$$.      Uptime: 17m
  ',$$P       ,ggs.     `$$b:    Packages: 2065
  `d$$'     ,$P"'   .    $$$     Shell: bash
   $$P      d$'     ,    $$P     Resolution: 1920x1200
   $$:      $$.   -    ,d$$'     DE: KDE 5.103.0 / Plasma 5.27.2
   $$\;      Y$b._   _,d$P'      WM: KWin
   Y$$.    `.`"Y$$$$P"'          GTK Theme: Breeze [GTK2/3]
   `$$b      "-.__               Icon Theme: breeze
    `Y$$                         Disk: 501G / 1.1T (48%)
     `Y$$.                       CPU: AMD Ryzen 9 5950X 16-Core @ 32x 3.4GHz
       `$$b.                     GPU: VMware SVGA II Adapter
         `Y$$b.                  RAM: -
            `"Y$b._             
                `""""           
                                
```
按提示打开/usr/bin/screenfetch这个脚本，可以看到报错位置的上下文是这样的：

```text
mem=$(free -b | awk -F ':' 'NR==2{print $2}' | awk '{print $1"-"$6}')
usedmem=$((mem / 1024 / 1024))
```
运行free -b命令，可见正常输出：
```text
               total        used        free      shared  buff/cache   available
内存：   16781590528  3409371136 11827744768   169746432  2212745216 13372219392
交换：    1023406080           0  1023406080
```
但是运行 free -b | awk -F ':' 'NR==2{print $2}' 就无输出了。
抓耳挠腮的想了一下，才发现在中文的debian系统上，free -b命令所输出内容中的冒号是中文字符，而screenfetch脚本中匹配处理的是英文字符。
进行如下丑陋的打补丁后，报错问题解决：
```text
# 这是一个丑陋的补丁，用以解决中文系统下的符号识别问题
# mem=$(free -b | awk -F ':' 'NR==2{print $2}' | awk '{print $1"-"$6}')
mem=$(free -b | awk -F '：' 'NR==2{print $2}' | awk '{print $1"-"$6}')
usedmem=$((mem / 1024 / 1024))
```

[原文链接](https://www.cnblogs.com/songxi/p/17304225.html)