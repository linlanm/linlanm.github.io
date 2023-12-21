---
title: Homebrew安装及配置
date: 2023-12-22 00:24:57
tags:
- macOS
categories:
- macOS
---

### 首次安装 Homebrew / Linuxbrew

首先，需要确保系统中安装了 bash、git 和 curl，对于 macOS 用户需额外要求安装 Command Line Tools (CLT) for Xcode。

*   对于 macOS 用户，系统自带 bash、git 和 curl，在命令行输入 `xcode-select --install` 安装 CLT for Xcode 即可。
*   对于 Linux 用户，系统自带 bash，仅需额外安装 git 和 curl。

接着，在终端输入以下几行命令设置环境变量：

```
export HOMEBREW_INSTALL_FROM_API=1
export HOMEBREW_API_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles"
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"
```

*注：自 `brew` 4.0 起，`HOMEBREW_INSTALL_FROM_API` 会成为默认行为，无需设置；大部分用户无需再克隆 homebrew/core 仓库，故无需设置 `HOMEBREW_CORE_GIT_REMOTE` 环境变量。但若需要运行 `brew` 的开发命令或者 `brew` 安装在非官方支持的默认 prefix 位置，则仍需设置 `HOMEBREW_CORE_GIT_REMOTE` 环境变量；如果不想通过 API 安装，可以设置 `HOMEBREW_NO_INSTALL_FROM_API=1`。*

最后，在终端运行以下命令以安装 Homebrew / Linuxbrew：

```
# 从本镜像下载安装脚本并安装 Homebrew / Linuxbrew
git clone --depth=1 https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/install.git brew-install
/bin/bash brew-install/install.sh
rm -rf brew-install

# 也可从 GitHub 获取官方安装脚本安装 Homebrew / Linuxbrew
/bin/bash -c "$(curl -fsSL https://github.com/Homebrew/install/raw/master/install.sh)"
```

这样在首次安装的时候也可以使用镜像。更多信息请参考 [Homebrew 官方安装文档](https://docs.brew.sh/Installation)。

*\* 安装成功后需将 brew 程序的相关路径加入到环境变量中：*

*   *以下针对基于 Apple Silicon CPU 设备上的 macOS 系统（命令行运行 `uname -m` 应输出 `arm64`）上的 Homebrew：*
    
    ```
    test -r ~/.bash_profile && echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.bash_profile
    test -r ~/.zprofile && echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
    ```
    
    *对基于 Intel CPU 设备上的 macOS 系统（命令行运行 `uname -m` 应输出 `x86_64`）的用户可跳过本步。*
    
*   *以下针对 Linux 系统上的 Linuxbrew：*
    
    ```
    test -d ~/.linuxbrew && eval "$(~/.linuxbrew/bin/brew shellenv)"
    test -d /home/linuxbrew/.linuxbrew && eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
    test -r ~/.bash_profile && echo "eval \"\$($(brew --prefix)/bin/brew shellenv)\"" >> ~/.bash_profile
    test -r ~/.profile && echo "eval \"\$($(brew --prefix)/bin/brew shellenv)\"" >> ~/.profile
    test -r ~/.zprofile && echo "eval \"\$($(brew --prefix)/bin/brew shellenv)\"" >> ~/.zprofile
    ```
    
    *参考了 [https://docs.brew.sh/Homebrew-on-Linux](https://docs.brew.sh/Homebrew-on-Linux)。*
    

### 替换现有仓库上游

替换 brew 程序本身的源，Homebrew / Linuxbrew 相同：

```
export HOMEBREW_API_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api"
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"
brew update
```

以下针对 macOS 系统上的 Homebrew：

```
# 手动设置
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"

# 注：自 brew 4.0 起，大部分 Homebrew 用户无需设置 homebrew/core 和 homebrew/cask 镜像，只需设置 HOMEBREW_API_DOMAIN 即可。
# 如果需要使用 Homebrew 的开发命令 (如 `brew cat <formula>`)，则仍然需要设置 homebrew/core 和 homebrew/cask 镜像。
# 请按需执行如下两行命令：
brew tap --custom-remote --force-auto-update homebrew/core https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
brew tap --custom-remote --force-auto-update homebrew/cask https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git

# 除 homebrew/core 和 homebrew/cask 仓库外的 tap 仓库仍然需要设置镜像
brew tap --custom-remote --force-auto-update homebrew/cask-fonts https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask-fonts.git
brew tap --custom-remote --force-auto-update homebrew/cask-versions https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask-versions.git
brew tap --custom-remote --force-auto-update homebrew/command-not-found https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-command-not-found.git
brew tap --custom-remote --force-auto-update homebrew/services https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-services.git
brew update

# 或使用下面的几行命令自动设置
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"
for tap in core cask{,-fonts,-versions} command-not-found services; do
    brew tap --custom-remote --force-auto-update "homebrew/${tap}" "https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-${tap}.git"
done
brew update
```

以下针对 Linux 系统上的 Linuxbrew：

```
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"

# 注：自 brew 4.0 起，使用默认 prefix (即 "/home/linuxbrew/.linuxbrew") 的大部分 Homebrew 用户无需设置 homebrew/core 镜像，只需设置 HOMEBREW_API_DOMAIN 即可。
# 如果不是默认 prefix 或者需要使用 Homebrew 的开发命令 (如 `brew cat <formula>`)，则仍然需要设置 homebrew/core 镜像。
# 请按需执行如下命令：
brew tap --custom-remote --force-auto-update homebrew/core https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git

# 除 homebrew/core 仓库外的 tap 仓库仍然需要设置镜像
brew tap --custom-remote --force-auto-update homebrew/command-not-found https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-command-not-found.git
brew tap --custom-remote --force-auto-update homebrew/services https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-services.git
brew update
```

**注：如果用户设置了环境变量 `HOMEBREW_BREW_GIT_REMOTE` 和 `HOMEBREW_CORE_GIT_REMOTE`，则每次执行 `brew update` 时，`brew` 程序本身和 Core Tap (`homebrew-core`) 的远程将被自动设置。推荐用户将这两个环境变量设置加入 shell 的 profile 设置中。**

```
test -r ~/.bash_profile && echo 'export HOMEBREW_API_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api"' >> ~/.bash_profile  # bash
test -r ~/.bash_profile && echo 'export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"' >> ~/.bash_profile  # bash
test -r ~/.bash_profile && echo 'export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"' >> ~/.bash_profile
test -r ~/.profile && echo 'export HOMEBREW_API_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api"' >> ~/.profile
test -r ~/.profile && echo 'export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"' >> ~/.profile
test -r ~/.profile && echo 'export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"' >> ~/.profile

test -r ~/.zprofile && echo 'export HOMEBREW_API_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api"' >> ~/.zprofile  # zsh
test -r ~/.zprofile && echo 'export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"' >> ~/.zprofile  # zsh
test -r ~/.zprofile && echo 'export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"' >> ~/.zprofile
```

### Linuxbrew 镜像迁移说明

Linuxbrew 核心仓库（`linuxbrew-core`）自 2021 年 10 月 25 日（`brew` 版本 3.3.0 起）被弃用，Linuxbrew 用户应迁移至 `homebrew-core`。Linuxbrew 用户请依新版镜像说明重新设置镜像。注意迁移前请先运行 `brew update` 将 `brew` 更新至 3.3.0 或以上版本。迁移过程中若出现任何问题，可使用如下命令重新安装 `homebrew-core`：

```
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"
rm -rf "$(brew --repo homebrew/core)"
brew tap --custom-remote --force-auto-update homebrew/core https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
```

### 复原仓库上游

*(感谢 Snowonion Lee 提供说明)*

*   以下针对 macOS 系统上的 Homebrew

```
# brew 程序本身，Homebrew / Linuxbrew 相同
unset HOMEBREW_BREW_GIT_REMOTE
git -C "$(brew --repo)" remote set-url origin https://github.com/Homebrew/brew

# 以下针对 macOS 系统上的 Homebrew
unset HOMEBREW_API_DOMAIN
unset HOMEBREW_CORE_GIT_REMOTE
BREW_TAPS="$(BREW_TAPS="$(brew tap 2>/dev/null)"; echo -n "${BREW_TAPS//$'\n'/:}")"
for tap in core cask{,-fonts,-versions} command-not-found services; do
    if [[ ":${BREW_TAPS}:" == *":homebrew/${tap}:"* ]]; then  # 只复原已安装的 Tap
        brew tap --custom-remote "homebrew/${tap}" "https://github.com/Homebrew/homebrew-${tap}"
    fi
done

# 重新拉取远程
brew update
```

*   以下针对 Linux 系统上的 Linuxbrew

```
# brew 程序本身，Homebrew / Linuxbrew 相同
unset HOMEBREW_BREW_GIT_REMOTE
git -C "$(brew --repo)" remote set-url origin https://github.com/Homebrew/brew

# 以下针对 Linux 系统上的 Linuxbrew
unset HOMEBREW_API_DOMAIN
unset HOMEBREW_CORE_GIT_REMOTE
brew tap --custom-remote homebrew/core https://github.com/Homebrew/homebrew-core
brew tap --custom-remote homebrew/command-not-found https://github.com/Homebrew/homebrew-command-not-found
brew tap --custom-remote homebrew/services https://github.com/Homebrew/homebrew-services

# 重新拉取远程
brew update
```

**注：重置回默认远程后，用户应该删除 shell 的 profile 设置中的环境变量 `HOMEBREW_BREW_GIT_REMOTE` 和 `HOMEBREW_CORE_GIT_REMOTE` 以免运行 `brew update` 时远程再次被更换。**

[原文链接](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/)