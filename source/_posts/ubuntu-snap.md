---
title: Ubuntu禁用snap
date: 2026-09-04 12:03:55
tags:
- Linux
categories:
- Linux
---

> ⚠️注意：卸载 snapd 会删掉 snap‑firefox，之后需要手动装 deb 版火狐；软件中心也会受影响。适用于 24.04 / 26.04。

## 步骤 1：先删除所有已安装 snap 应用

```
# 查看全部snap
snap list
```

```
# 删除除snapd以外全部snap包
sudo snap remove --purge $(snap list | awk 'NR>1 && $1!="snapd" {print $1}')
# 最后删除snapd自身snap
sudo snap remove --purge snapd
```

> 执行完 `snap list` 提示：`No snaps are installed yet.` 才算清空。

## 步骤 2：停止并屏蔽 snapd systemd 服务

```
sudo systemctl stop snapd.service snapd.socket snapd.seeded.service
sudo systemctl disable --now snapd.service snapd.socket snapd.seeded.service
sudo systemctl mask snapd
```

## 步骤 3：apt 卸载 snapd deb 包

```
sudo apt purge -y snapd snap-confine gnome-software-plugin-snap
sudo apt autoremove --purge
```

## 步骤 4：清理残留目录

```
sudo rm -rf /snap /var/snap /var/lib/snapd /var/cache/snapd
rm -rf ~/snap
```

## 步骤 5：【最重要】阻止 apt 偷偷重装 snapd

创建 apt 屏蔽规则，防止升级 / 装软件时自动拉回 snapdGitHub：

```
sudo tee /etc/apt/preferences.d/nosnap.pref <<'EOF'
Package: snapd
Pin: release a=*
Pin-Priority: -10

Package: snap-confine
Pin: release a=*
Pin-Priority: -10
EOF
```

```
sudo apt update
```

> 此时 `sudo apt install snapd` 会直接提示包不可安装。

## 步骤 6：替换 Firefox（deb 版本）

Ubuntu 默认 firefox deb 是空包装，会拉 snap。需要添加 Mozilla 官方源安装真正 deb 版 firefox：

```
# 删除旧空包
sudo apt purge firefox
```

```
# 添加Mozilla APT源
sudo add-apt-repository ppa:mozillateam/ppa
sudo apt update
sudo apt install firefox
```

## 验证是否彻底禁用

```
which snap
# 无输出说明成功
systemctl status snapd
# 显示 masked
apt install snapd
# 提示包不可安装
```

* * *

## 如果只是临时禁用，不想彻底删除（不推荐）

仅停止屏蔽服务，不删包：

```
sudo systemctl mask snapd
```

> 缺点：apt 升级很可能把服务恢复回来，治标不治本。