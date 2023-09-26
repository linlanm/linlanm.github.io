---
title: hexo-分类&标签应用
date: 2023-09-26 18:42:32
tags:
- Hexo
categories:
- Hexo
---


> 指令解析


根据scaffolds模板下的page文件创建相应的分类或者标签概念
```
hexo new page xxx
```
该指令执行完成会在source下相应生成一个xxx文件夹（内有一个index.md文件）


## 1.分类&标签创建

### 分类创建

> 创建分类页面


执行命令创建分类
```
hexo new page categories
```

构建完成：categories/index.md对应内容

```
title: categories
date: xxxx-xx-xx xx:xx:xx
```

修改index.md文件，将页面类型设置为 categories ，主题将自动为这个页面显示所有分类（可根据自身需求配置相关内容）

```
title: categories
date: xxxx-xx-xx xx:xx:xx
type: "categories"
```

### 标签创建

> 创建标签页面

执行命令创建标签
```
hexo new page tags
```

构建完成：tags/index.md对应内容

```
title: tags
date: xxxx-xx-xx xx:xx:xx
```

改index.md文件，将页面类型设置为 tags，主题将自动为这个页面显示所有标签（可根据自身需求配置相关内容）

```
title: tags
date: xxxx-xx-xx xx:xx:xx
type: "tags"
```

## 2.分类&标签应用

#### 在文章中配置分类和标签

​创建一个文章，编辑front-matter，设定相应的分类和标签信息，注意区分分类、标签的层次关系
​只有文章支持分类和标签，可以在 Front-matter 中设置。在其他系统中，分类和标签听起来很接近，但是在 Hexo 中两者有着明显的差别：分类具有顺序性和层次性；而标签没有顺序和层次。

```
title: 个人博客
categories: 
    - java-note
    - base
tags: 
    - java
    - database
date: xxx-xx-xx xx:xx:xx
updated: xxx-xx-xx xx:xx:xx
```

​ 预览文章内容，显示信息。

#### 多级分类构建

> 多级分类构建

```
categories:
    - [java-note, base]
    - [java-note, database]
    - [vue-note]
```
上述分类为：java-note下有base、database两个子分类，而vue-note没有子分类。