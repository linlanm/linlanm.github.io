---
title: ArchLinux安装教程
date: 2023-09-26 17:45:01
tags:
- 系统安装
- Linux
categories:
- Linux
---
**一、准备工作**

1.下载arch最新安装镜像;

2.烧录镜像至U盘（4G或8G）

Linux:

`$ sudo dd bs=4M if=archlinux-2020.01.01-x86_64.iso of=/dev/sdc` 

Windows:  
使用rufus,模式选择dd

**二、启动到live环境**

**1.进入装机环境**

**2.验证启动模式**  
`# ls /sys/firmware/efi/efivars`  
如果目录不为空，说明当前系统是以UEFI模式启动的，如果目录不存在，系统可能以 BIOS或 CSM 模式启动

**3.连接到互联网**  
a.启动dhcp服务  
`# systemctl start dhcpcd`  
b.查看网络连接  
`# ip a`  
c.检查网络连通性  
`# ping -c 3 www.baidu.com`

**4.更新系统时间**  
`# timedatectl set-ntp true`

**5.硬盘分区**

**查看硬盘信息**  
`# lsblk`

**分区示例**(磁盘以sda为例)

**BIOS和MBR**

| 挂载点 | 分区 | 分区类型 | 建议大小 |
| --- | --- | --- | --- |
| /mnt | /dev/sda1 | Linux | 剩余空间 |
| \[SWAP\] | /dev/sda2 | Linux swap (交换空间) | 大于 512 MiB |

**UEFI with GPT**

| 挂载点 | 分区 | 分区类型 | 建议大小 |
| --- | --- | --- | --- |
| /mnt/boot/efi  | /dev/sda1 | EFI 系统分区 | 260–512 MiB |
| /mnt | /dev/sda2 | Linux x86-64 根目录 (/) | 剩余空间 |
| \[SWAP\] | /dev/sda3 | Linux swap (交换空间) | 大于 512 MiB |

*   请使用 cfdisk 或 parted 修改分区表，例如 `# cfdisk /dev/sda`
*   如果文件系统支持，交换空间也可以设在交换文件上。

**6.格式化分区**(磁盘以sda为例)  
当分区建立好了，这些分区都需要使用适当的文件系统进行格式化。举例,如果根分区在 /dev/sda2 上并且会使用 *ext4* 文件系统，efi分区在/dev/sda1上，运行：  
`# mkfs.fat -F32 /dev/sda1` efi分区  
`# mkfs.*ext4* /dev/sda2` /分区  
如果您创建了交换分区（例如 /dev/sda3），使用 mkswap 将其初始化：  
`# mkswap /dev/sda3`  
`# swapon /dev/sda3`

**7.挂载分区**(磁盘以sda为例)  
将根分区挂载到 `/mnt`，例如：  
`# mount /dev/sda2 /mnt`  
创建其他剩余的挂载点（比如 `/mnt/boot/efi`）并挂载其相应的分区。  
`# mkdir /mnt/boot/efi -p   # mount /dev/sda1 /mnt/boot/efi`  
接下来 genfstab 将会自动检测挂载的文件系统和交换空间。

**三、安装系统**

**1.选择镜像**  
文件 `/etc/pacman.d/mirrorlist` 定义了软件包会从哪个镜像源下载。在 LiveCD 启动的系统上，所有的镜像都被启用，并且在镜像被制作时，我们已经通过他们的同步情况和速度排序。  
在列表中越前的镜像在下载软件包时有越高的优先权。你可以相应的修改文件 `/etc/pacman.d/mirrorlist`，并**将地理位置最近的镜像源挪到文件的头部**，同时你也应该考虑一些其他标准。  
这个文件接下来还会被 *pacstrap* 拷贝到新系统里，所以请确保设置正确。  
例：  
`# vim /etc/pacman.d/mirrorlist`  
```
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
```

**2.安装必须的软件包**  
`# pacstrap /mnt base base-devel linux linux-firmware vim`

**四、配置系统**

**1.Fstab**  
用以下命令生成fstab文件 (用 `-U` 或 `-L` 选项设置UUID 或卷标)：  
`# genfstab -U /mnt >> /mnt/etc/fstab`  
**强烈建议** 在执行完以上命令后，后检查一下生成的 `/mnt/etc/fstab` 文件是否正确。

**2.Chroot**  
Change root 到新安装的系统：  
`# arch-chroot /mnt`

**3.设置时区**  
`# ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`  
`# hwclock --systohc --localtime`

**4.本地化**  
本地化的程序要本地化文本，都依赖 Locale，后者明确规定地域、货币、时区日期的格式、字符排列方式和其他本地化标准等等。在下面两个文件设置：`locale.gen` 与 `locale.conf`。  
`/etc/locale.gen` 是一个仅包含注释文档的文本文件。指定您需要的本地化类型，只需移除对应行前面的注释符号（`＃`）即可，建议选择带 `UTF-8` 的项：  
`# vim /etc/locale.gen`  
```
en_US.UTF-8 UTF-8  
zh_CN.UTF-8 UTF-8  
zh_TW.UTF-8 UTF-8
```
接着执行 `locale-gen` 以生成 locale 信息：  
`# locale-gen`

`/etc/locale.gen` 会生成指定的本地化文件。  
创建 `locale.conf` 并编辑 `LANG` 这一变量,比如将系统 locale 设置为 `en_US.UTF-8`，系统的 Log 就会用英文显示，这样更容易问题的判断和处理。  
`# echo LANG=en_US.UTF8 > /etc/locale.conf`  
**警告:** 不推荐在此设置任何中文 locale，会导致 TTY 乱码。

**5.设置网络**  
a.创建 hostname 文件  
`# vim /etc/hostname`  
```
myhostname
```

b.添加对应的信息到 hosts:  
`# vim /etc/hosts`  
```
127.0.0.1 localhost  
::1 localhost  
127.0.1.1 myhostname.localdomain myhostname
```
**6.用户配置**  
a.修改root密码  
`# passwd root`  
b.创建新用户(merlin是新建的用户名)  
`# useradd -m -G wheel merlin`  
c.修改用户密码  
`# passwd merlin`  
d.修改sudo权限  
`# visudo`  
去掉`# %wheel ALL=(ALL) ALL` 前面的注释

**7.其它配置**  
a.*有线连接*  
`# systemctl enable dhcpcd`   开机自动启动dhcp服务  
如果上述命令执行提示错误，执行`# pacman -S dhcpcd`  
b***.***无线连接  
`# pacman -S iw wpa_supplicant dialog`

**8.按照系统引导**(sda修改为实际安装磁盘)

***BIOS+BMR分区方案引导系统启动***  
```
# pacman -S grub   
# grub-install --target=i386-pc /dev/sda   
# grub-mkconfig -o /boot/grub/grub.cfg
```
**UEFI+*GPT分区方案引导系统启动***  
```
# pacman -S grub efibootmgr   
# grub-install --target=x86_64-efi /dev/sda    
# grub-mkconfig -o /boot/grub/grub.cfg
```

**五、重启**

输入 `exit` 或按 `Ctrl+d` 退出 chroot 环境。  
可选用 `umount -R /mnt` 手动卸载被挂载的分区：这有助于发现任何「繁忙」的分区，并通过 [fuser(1)](https://jlk.fjfi.cvut.cz/arch/manpages/man/fuser.1) 查找原因。  
最后，通过执行 `reboot` 重启系统，*systemd* 将自动卸载仍然挂载的任何分区。不要忘记移除安装介质，然后使用 root 帐户登录到新系统。

**注意**：本文章基于Arch Wiki修改，[原文链接](https://wiki.archlinux.org/index.php/Installation_guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))