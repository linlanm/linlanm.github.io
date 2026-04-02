---
title: ADB 命令大全（分类速查版）
date: 2026-04-02 23:59:46
tags:
- ADB
categories:
- ADB
---

这是一份**ADB 命令大全（分类速查版）**，覆盖日常调试、自动化、系统控制、性能、日志、文件、应用、网络、权限、模拟输入、刷机等场景，直接复制可用。

### 📱 设备连接与基础

```
# 查看设备列表
adb devices
# 查看设备状态
adb get-state
# 重启ADB服务
adb kill-server && adb start-server
# 无线连接（先USB，再执行）
adb tcpip 5555
adb connect 设备IP:5555
# 断开无线连接
adb disconnect 设备IP:5555
# 指定设备操作（多设备）
adb -s 设备ID 命令
# 重启设备
adb reboot
# 重启到Recovery
adb reboot recovery
# 重启到Bootloader
adb reboot bootloader
```

### 📦 应用管理（pm /am）

```
# 安装APK
adb install app.apk
# 覆盖安装（保留数据）
adb install -r app.apk
# 允许降级安装
adb install -d app.apk
# 卸载应用
adb uninstall 包名
# 卸载但保留数据
adb uninstall -k 包名
# 列出所有包名
adb shell pm list packages
# 列出系统应用
adb shell pm list packages -s
# 列出第三方应用
adb shell pm list packages -3
# 按关键字过滤包名
adb shell pm list packages | grep 关键词
# 查看应用安装路径
adb shell pm path 包名
# 清除应用数据与缓存
adb shell pm clear 包名
# 启动应用
adb shell am start -n 包名/启动Activity
# 强制停止应用
adb shell am force-stop 包名
# 杀死应用进程（系统可重启）
adb shell am kill 包名
# 发送广播（示例：重启）
adb shell am broadcast -a android.intent.action.REBOOT
```

### 📂 文件传输与管理

```
# 电脑→设备
adb push 本地路径 设备路径
# 设备→电脑
adb pull 设备路径 本地路径
# 进入设备Shell
adb shell
# 查看目录
adb shell ls /sdcard
# 创建目录
adb shell mkdir /sdcard/test
# 删除文件
adb shell rm /sdcard/file.txt
# 递归删除目录
adb shell rm -r /sdcard/test
# 复制文件
adb shell cp 源 目标
# 移动/重命名
adb shell mv 源 目标
# 修改权限
adb shell chmod 755 /sdcard/file.sh
```

### 📷 截图 / 录屏

```
# 截图并保存到设备
adb shell screencap /sdcard/screen.png
# 截图并直接保存到电脑（一步到位）
adb exec-out screencap -p > screen.png
# 录屏（默认3分钟，Ctrl+C停止）
adb shell screenrecord /sdcard/record.mp4
# 录屏并指定时长（秒）
adb shell screenrecord --time-limit 60 /sdcard/record.mp4
```

### 📝 日志（logcat）

```
# 实时日志
adb logcat
# 清空日志缓存
adb logcat -c
# 带时间戳并保存到文件
adb logcat -v time > log.txt
# 过滤Error级别日志
adb logcat *:E
# 过滤指定包名的Error日志
adb logcat --pid=$(adb shell pidof 包名) *:E
# 过滤含关键词的日志
adb logcat | grep "关键词"
# 导出完整系统报告（含日志、状态）
adb bugreport > bugreport.zip
```

### 🛠 系统信息与调试

```
# 查看所有系统属性
adb shell getprop
# 查看特定属性（如Android版本）
adb shell getprop ro.build.version.release
# 查看屏幕分辨率
adb shell wm size
# 查看屏幕密度
adb shell wm density
# 查看所有系统服务信息
adb shell dumpsys
# 查看Activity栈（当前页面）
adb shell dumpsys activity activities | grep mResumedActivity
# 查看CPU信息
adb shell dumpsys cpuinfo
# 查看内存信息（指定包）
adb shell dumpsys meminfo 包名
# 查看电池状态
adb shell dumpsys battery
# 查看进程（top5）
adb shell top -m 5
# 查看设备时间
adb shell date
```

### 🎮 模拟输入（input）

```
# 点击坐标(x,y)
adb shell input tap x y
# 滑动（起点x1,y1 → 终点x2,y2）
adb shell input swipe x1 y1 x2 y2
# 输入文本
adb shell input text "hello"
# 按返回键
adb shell input keyevent 4
# 按Home键
adb shell input keyevent 3
# 按电源键
adb shell input keyevent 26
# 按音量+
adb shell input keyevent 24
```

### 🔌 网络与端口

```
# 端口转发（电脑5555→设备8888）
adb forward tcp:5555 tcp:8888
# 查看转发列表
adb forward --list
# 删除转发
adb forward --remove tcp:5555
# 查看设备IP
adb shell ifconfig
# 查看网络连接
adb shell netstat
```

### 🔐 权限与 Root

```
# 获取Root权限（设备已Root）
adb root
# 重新挂载系统分区（可写）
adb remount
# 授予运行时权限
adb shell pm grant 包名 android.permission.WRITE_EXTERNAL_STORAGE
# 撤销运行时权限
adb shell pm revoke 包名 android.permission.WRITE_EXTERNAL_STORAGE
```

### 🧪 性能与压力测试（Monkey）

```
# 对指定应用做1000次随机事件
adb shell monkey -p 包名 -v 1000
# 忽略崩溃与超时
adb shell monkey -p 包名 --ignore-crashes --ignore-timeouts -v 5000
```

### ⚡ Fastboot（线刷模式）

```
# 查看Fastboot设备
fastboot devices
# 刷入boot镜像
fastboot flash boot boot.img
# 刷入系统镜像
fastboot flash system system.img
# 解锁Bootloader
fastboot oem unlock
# 重启设备
fastboot reboot
```

* * *

