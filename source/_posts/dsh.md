---
title: Intel Mac手动部署安装dsh
date: 2026-11-14 14:30:03
tags:
- AI
categories:
- AI
---

# DeepSeek Harness



DeepSeek Harness（`dsh`）是由 [DeepSeek AI](https://deepseek.com/) 开发的开源 agent harness（智能体框架）。

它构建于**一切皆插件**的架构之上，由 [Cordis](https://github.com/cordiverse/cordis) 驱动，其设计参见论文 [*A Programming Paradigm for Spatiotemporal Composability*](https://arxiv.org/abs/2608.25512)。

文档：[https://deepseek-harness.github.io/deepseek-harness/](https://deepseek-harness.github.io/deepseek-harness/)

## 开发者预览

[](https://github.com/deepseek-ai/deepseek-harness/blob/master/README.zh.md#%E5%BC%80%E5%8F%91%E8%80%85%E9%A2%84%E8%A7%88)

DeepSeek Harness 处于 *开发者预览* 阶段，正在快速迭代。**未来将出现破坏兼容性的变更。**

运行本项目前，请阅读[安全说明](https://github.com/deepseek-ai/deepseek-harness/blob/master/SAFETY.zh.md)。

## 运行

[](https://github.com/deepseek-ai/deepseek-harness/blob/master/README.zh.md#%E8%BF%90%E8%A1%8C)

### 通过 `npm` 运行

[](https://github.com/deepseek-ai/deepseek-harness/blob/master/README.zh.md#%E9%80%9A%E8%BF%87-npm-%E8%BF%90%E8%A1%8C)

安装 `Node.js`，然后运行：

```shell
npx @deepseek-ai/dsh web
```

该命令默认会在 `http://127.0.0.1:3080` 启动 Web UI，本机启动时还会用默认浏览器打开页面。通过 SSH 启动时只打印宿主机 URL，因为本地转发地址由 SSH 客户端或编辑器持有。传入 `--no-open` 可仅运行服务器而不打开浏览器。详见 [Web UI 指南](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/guide/index.zh.md)。

### 从源码运行

[](https://github.com/deepseek-ai/deepseek-harness/blob/master/README.zh.md#%E4%BB%8E%E6%BA%90%E7%A0%81%E8%BF%90%E8%A1%8C)

如需从仓库源码运行：

```shell
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install
pnpm run build
pnpm dsh web
```

`pnpm run build` 会准备仓库产物。`pnpm dsh web` 会直接使用这些已构建产物，不会重新构建。

## 常见问题
### 1."zsh: command not found: pnpm"

`zsh: command not found: pnpm` = **pnpm 没装，或者装了但路径没加到 zsh 的 PATH 里**（mac 默认 zsh）pnpm

#### 方案 1：官方一键安装（推荐 Mac/Linux）

```
curl -fsSL https://get.pnpm.io/install.sh | sh -
```

安装脚本会自动写入 `~/.zshrc` 配置 PNPM\_HOME 环境变量pnpm

安装完**必须新开终端**，或者手动生效：

```
source ~/.zshrc
```

验证：

```
pnpm -v
```

#### 方案 2：用 npm 全局安装（已有 node）

```
npm install -g pnpm
```

> ⚠️ 如果是 nvm 管理 node，一般直接可用；如果还是找不到，就是全局 bin 目录不在 PATH

#### 方案 3：已经装过、只是环境变量丢了（mac 最常见）

pnpm 默认 mac 路径：`~/Library/pnpm` 编辑 zsh 配置：

```
nano ~/.zshrc
```

加入下面这段：

```
export PNPM_HOME="$HOME/Library/pnpm"
export PATH="$PNPM_HOME:$PATH"
```

保存退出（nano：Ctrl+O 回车，Ctrl+X），然后生效：

```
source ~/.zshrc
```

#### 排查命令

```
# 看看有没有pnpm
which pnpm
# 查看当前PATH
echo $PATH
```

#### 国内加速小提示

安装慢可以换淘宝源：

```
npm config set registry https://registry.npmmirror.com
```

### 2.“npm error     at async /opt/local/lib/node_modules/npm/node_modules/@npmcli/arborist/lib/arborist/reify.js:638:20 *** npm error permissions of the file and its containing directories, or try running npm error the command again as root/Administrator.“

你的 node 是装在 `/opt/local`（macports），这个目录属于 root，普通用户没有写入权限，所以 `npm install -g` 直接报错 EACCES。

> ❌ **不推荐直接 sudo npm install -g pnpm**，会埋下一堆权限坑，后面各种诡异报错。

#### ✅ 方案 1（首选，最简单，推荐）：pnpm 官方独立安装脚本

它直接装到你家目录 `~/Library/pnpm`，完全避开 `/opt/local` 权限问题：

```
curl -fsSL https://get.pnpm.io/install.sh | sh -
```

脚本会自动写入 zsh 配置。 执行完：

```
source ~/.zshrc
pnpm -v
```

能输出版本号就成功了。

> 如果国内下载慢，可以用镜像版：

```
curl -fsSL https://get.pnpm.io/install.sh | env PNPM_DIST_URL=https://registry.npmmirror.com/pnpm | sh -
```

#### ✅ 方案 2：brew 安装（如果你有 homebrew）

```
brew install pnpm
```

#### ✅ 方案 3：如果你坚持想用 npm 全局装（改 npm 全局目录，一劳永逸解决所有 npm 全局 EACCES）

```
# 1. 创建用户目录存放全局包
mkdir ~/.npm-global
# 2. 修改npm全局安装路径
npm config set prefix '~/.npm-global'
# 3. 写入zshrc
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.zshrc
# 4. 生效
source ~/.zshrc
# 现在再安装就不会有权限报错
npm install -g pnpm
```

#### ✅ 方案 4：用 nvm 管理 node（根治这类权限问题，长期开发强烈建议）

macports /pkg 直接装 node 很容易出现 `/opt/local`、`/usr/local` 权限问题，换成 nvm，所有 node 版本、全局包全部在用户目录，再也不会 EACCES。

* * *

#### 小排查

装好之后如果还是 command not found：

```
cat ~/.zshrc | grep PNPM_HOME
```

正常会看到两行：

```
export PNPM_HOME="$HOME/Library/pnpm"
export PATH="$PNPM_HOME:$PATH"
```

没有的话手动加上再 `source ~/.zshrc`


### 3.pnpm install Error: ERR\_PNPM\_PNPM\_ENGINE\_NO\_NATIVE\_BINARY

报错根源：
你是 **Intel Mac（darwin-x64）**，**pnpm 11 独立版（@pnpm/exe）没有 Intel Mac 的二进制包**，官方直接砍掉了，所以项目里 `packageManager` 指定了 pnpm@11，pnpm 尝试自动下载 @pnpm/exe 就炸了pnpm。

> 一句话：**pnpm v11 独立 exe 不支持 Intel Mac**，M 芯片没问题，Intel 不行。

#### 方案 A（最快临时绕过，直接跑）

项目里强制忽略版本锁定，用你本地装好的 pnpm：

```
pnpm with current install
```

或者一次性环境变量：

```
PNPM_PM_ON_FAIL=ignore pnpm install
```

永久忽略：在项目根新建 / 编辑 `pnpm-workspace.yaml`

```
pmOnFail: ignore
```

⚠️ 缺点：项目要求的 pnpm 版本约束失效，适合本地开发，不要提交到公共仓库。

#### 方案 B（推荐，Intel Mac 稳定可用）

#### 方式 1：Homebrew 安装（优先）

```
brew install pnpm
```

brew 装的是 node 版 pnpm（不是 @pnpm/exe 独立包），可以在 Intel Mac 正常跑，不受这个限制pnpm。

#### 方式 2：用 npm 装普通版 pnpm（不是 @pnpm/exe）

> 注意：是 `pnpm` 包，不是 `@pnpm/exe` 先用 nvm 管理 node（避开 /opt/local 权限坑），然后：

```
npm install -g pnpm
```

这个版本依赖本机 node，**不需要独立二进制，Intel Mac 可以正常运行**。

#### 方式 3：升级 pnpm 12（12 重新支持 Intel Mac 独立二进制）pnpm

```
curl -fsSL https://get.pnpm.io/install.sh | env PNPM_VERSION=12 sh -
```

> pnpm12 是 Rust 重写，重新提供 darwin-x64 原生包，Intel Mac 可以用独立版。

#### 方案 C：改项目 package.json（根治）

打开 `package.json`，找到类似：

```
"packageManager": "pnpm@11.28.5"
```

改成 pnpm 12，或者直接删掉这一行，就不会自动拉 @pnpm/exe。

#### 补充说明

*   `@pnpm/exe` = 打包了 node 的独立可执行文件，v11 不给 Intel Mac 做这个包了（Node SEA 在 x64 macOS 上会段错误）
*   不带 exe 的普通 `pnpm` npm 包：只是 cli 源码，跑在你本机 node 上，Intel Mac 一直可用
*   你之前的 `curl install.sh` 装的就是 @pnpm/exe 独立版，Intel Mac 11 版本踩坑是必然

#### 推荐顺序

1.  先用 `pnpm with current install` 把项目装起来
2.  长期：brew install pnpm 或者 装 pnpm12


### 4.脚本执行 `git rev-parse HEAD` 返回退出码 **128**
脚本执行 `git rev-parse HEAD` 返回退出码 **128**，含义：

> 当前目录**不是一个 git 仓库，或者 git 仓库没有任何 commit（空仓库）**

脚本里这个函数 `repositoryCommitHash` 想要读取当前 git 的 commit hash，但是找不到 git 仓库信息，直接抛出异常打断构建。

两种最常见情况：

1.  你解压下载源码包，**不是 git clone 下来的**，文件夹里没有 `.git` 目录
2.  是 git 仓库，但还没 add/commit，仓库是空的

* * *

#### 快速验证

在项目根目录执行：

```
git rev-parse HEAD
```

*   如果直接报 `fatal: not a git repository` → 就是情况 1
*   如果报 `fatal: ambiguous argument 'HEAD': unknown revision or path not in the working tree` → 空仓库

再检查有没有 `.git`：

```
ls -la .git
```

* * *

#### 方案 1（如果你是源码包，没有 git，想直接跳过 git 校验【最快】）

修改 `scripts/client-build-environment.ts`，找到这一行（大概 55 行）：

```
const hash = execFileSync('git', ['rev-parse', 'HEAD'], { encoding: 'utf8' }).trim();
```

包一层 try-catch，给一个默认值，示例：

```
let hash: string;
try {
  hash = execFileSync('git', ['rev-parse', 'HEAD'], { encoding: 'utf8' }).trim();
} catch (e) {
  hash = 'unknown';
}
```

保存后重新跑：

```
pnpm run build
```

#### 方案 2（正规方式，把这个目录初始化为 git 仓库） 推荐

项目根执行：

```
git init
git add .
git commit -m "init"
```

commit 之后 HEAD 就存在了，再 build：

```
pnpm run build
```

#### 方案 3（不想改代码、不想初始化 git，临时 mock）

如果你只是打包本地版本，不想提交代码，直接初始化一次 git 即可，不会影响源码：

```
git init && git add . && git commit -m "build-temp"
```

* * *


[原文链接](https://github.com/deepseek-ai/deepseek-harness/blob/master/README.zh.md)