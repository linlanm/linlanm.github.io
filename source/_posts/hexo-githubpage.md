---
title: hexo github搭建个人博客
date: 2025-04-25 16:09:53
tags:
- Hexo
categories:
- Hexo
---


在搭建个人博客时，在GitHub上使用Hexo是一个非常流行和便捷的选择。Hexo是一个基于Node.js的静态站点生成器，它允许你轻松地创建和部署博客。以下是如何在GitHub上使用Hexo搭建个人博客的步骤：

### 1\. 安装Node.js

首先，你需要在你的计算机上安装Node.js。你可以从[Node.js官网](https://nodejs.org/)下载并安装适合你操作系统的版本。

### 2\. 安装Hexo

安装Node.js后，打开命令行工具（如Terminal、CMD或PowerShell），然后运行以下命令来全局安装Hexo：

```bash
npm install -g hexo-cli
```

### 3\. 初始化Hexo博客

创建一个新的文件夹作为你的博客项目，并进入该文件夹：

```bash
mkdir my-blog
cd my-blog
```

然后，初始化你的Hexo博客：

```bash
hexo init
```

### 4\. 安装依赖

初始化完成后，安装博客所需的依赖：

```bash
npm install
```

### 5\. 本地预览

在本地启动Hexo服务器以预览你的博客：

```bash
hexo server
```

这将在`http://localhost:4000`启动一个本地服务器，你可以在浏览器中访问它来查看你的博客。

### 6\. 生成静态文件

当你对博客满意并准备部署时，生成静态文件：

```bash
hexo generate
```

这将在`public`文件夹中生成静态文件。

### 7\. 部署到GitHub Pages

#### 创建GitHub仓库

1.  在GitHub上创建一个新的仓库，仓库名称为`your-username.github.io`（确保你的用户名是正确的）。例如，如果用户名是`exampleuser`，则仓库名应该是`exampleuser.github.io`。
    
2.  将生成的`public`文件夹中的内容推送到这个仓库。首先，初始化本地仓库：
    
    ```bash
    cd publicgit initgit remote add origin https://github.com/your-username/your-username.github.io.gitgit add .git commit -m "First commit"git push -u origin master
    ```
    
    注意：从2021年1月开始，GitHub默认分支名称变为`main`，如果你使用的是新账户或新仓库，请使用`git push -u origin main`。
    

### 8\. 配置域名（可选）

如果你想要使用自定义域名（例如`yourname.com`），你需要在GitHub仓库的Settings中添加一个CNAME文件到你的仓库的根目录，然后在你的域名提供商处设置DNS记录指向`your-username.github.io`。

### 9\. 写作和更新博客

使用Hexo的Markdown编辑器写作：

```bash
hexo new "Your post name"
```

然后继续写作并使用`hexo generate`和`hexo deploy`命令来更新你的博客。

通过以上步骤，你就可以在GitHub上使用Hexo成功搭建并部署你的个人博客了。