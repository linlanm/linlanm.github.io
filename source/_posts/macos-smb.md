---
title: 调整macOS中的SMB浏览行为
date: 2023-12-05 20:09:05
tags:
- macOS
- SMB
categories:
- macOS
- SMB
---

用于浏览网络文件夹的默认设置（如服务器信息块 (SMB) 共享）适用于大多数组织和用户。尽管如此，你仍可以进行相应的调整来优化企业环境中的 SMB 浏览。

**本文适用于企业和教育机构的系统管理员。**

Mac 会收集相关的文件信息（如标签、标记以及其他形式的元数据），以此确定每个窗口及其内容的显示方式。

如果文件夹按字母数字进行排序，系统会立即显示相应的内容，然后“访达”会收集这个文件夹的剩余元数据并进行比较。

## 加快网络共享内容的浏览速度

要加快 SMB 文件的浏览速度，你可以阻止 macOS 读取 SMB 共享上的 .DS\_Store 文件。这样，“访达”将只使用基本信息来立即以字母数字顺序显示各个文件夹的内容。请使用以下终端命令：

`defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE`

然后退出登录 macOS 账户并重新登录。

要重新启用排序，请使用以下命令：

`defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool FALSE`

[原文链接](https://support.apple.com/zh-cn/HT208209)