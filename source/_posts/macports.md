---
title: MacPorts安装及配置
date: 2026-10-04 22:53:42
tags:
- macOS
- MacPorts
categories:
- macOS
- MacPorts
---

> MacPorts 是 macOS 的包管理器，和 Homebrew 并列；**安装前必须先装好 Xcode Command Line Tools**

## 1\. 前置依赖：安装 Xcode 命令行工具

打开终端（Terminal）执行：

```
xcode-select --install
```

弹出窗口点「安装」，等待下载完成。

> 如果提示已安装，直接跳过。

## 2\. 下载对应系统的 MacPorts pkg 安装包

官网：[https://www.macports.org/install.php](https://www.macports.org/install.php)The MacPor... 选择你的 macOS 版本：

*   Sequoia (15) / Sonoma (14) / Ventura (13) / Monterey (12) … 下载 `.pkg` 安装包，双击一路下一步安装。

> 安装程序会自动配置环境变量（`/opt/local/bin`），**安装完成必须新开终端窗口**，环境变量才生效。

## 3\. 验证是否安装成功

新开终端运行：

```
port version
```

输出版本号就代表安装成功。

> 如果提示 `command not found`：
> 
> *   zsh 用户：`echo 'export PATH="/opt/local/bin:/opt/local/sbin:$PATH"' >> ~/.zshrc`
> *   bash 用户：`echo 'export PATH="/opt/local/bin:/opt/local/sbin:$PATH"' >> ~/.bash_profile` 然后 `source ~/.zshrc` 重载配置

## 4\. 更新 ports 树（必做）

```
sudo port selfupdate
```

这一步拉取软件清单，国内网络会很慢，**建议换成清华源**。

## 5\. 【国内加速】切换清华镜像源

编辑 MacPorts 配置文件：

```
sudo nano /opt/local/etc/macports/sources.conf
```

把第一行

```
rsync://rsync.macports.org/macports/release/tarballs/ports.tar [default]
```

替换为清华源：

```
rsync://mirrors.tuna.tsinghua.edu.cn/macports/release/tarballs/ports.tar [default]
```

`Ctrl+O` 保存，回车；`Ctrl+X` 退出 nano。

再修改 distfiles（软件下载源）

```
sudo nano /opt/local/etc/macports/macports.conf
```

在文件末尾添加：

```
mirror_site https://mirrors.tuna.tsinghua.edu.cn/macports/distfiles/
```

保存退出。

再次执行更新：

```
sudo port selfupdate
```

## 6\. 常用基础命令

```
# 搜索软件
port search xxx

# 安装软件
sudo port install xxx

# 查看已安装
port installed

# 更新全部已安装软件
sudo port upgrade outdated

# 卸载
sudo port uninstall xxx

# 清理旧版本缓存
sudo port clean --all installed
```

## 7\. 常见问题

1.  **权限报错**：所有 port 安装更新都要加 `sudo`
2.  **黑苹果注意**：Xcode CLI 要匹配当前 macOS 版本，不要混用高版本 SDK
3.  **和 Homebrew 共存**：二者可以共存，注意 PATH 顺序，MacPorts 默认`/opt/local`，brew 在`/opt/homebrew`
4.  想卸载 MacPorts

```
sudo port -fp uninstall installed
sudo rm -rf /opt/local
sudo rm -rf ~/.macports
```

## 小提示

MacPorts 编译软件大多是**本地源码编译**，速度比 brew 慢，但依赖隔离做得很好，适合老 mac、黑苹果编译一些比较底层工具。