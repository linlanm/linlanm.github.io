---
title: Mac下删除Homebrew包及其依赖
date: 2024-01-01 23:02:31
tags:
- macOS
- Homebrew
categories:
- macOS
- Homebrew
---

假设现在有一个`Homebrew`包，然后希望删除该包和所有依赖项，但不包含也被其他包依赖的包。

## 安装命令

```ruby
$ brew tap beeftornado/rmtree
```

## 使用

```go
$ brew rmtree <package>
```

将上面的`<package>`换成想要删除的包即可。

## 示例
删除`mpv`这个包

```
$ brew rmtree mpv
==> Examining installed formulae required by mpv...
 -  43 / 43

Can safely be removed
----------------------
automake
lua
mpg123
mpv-player/mpv/libass-ct

Proceed?[y/N]: y
==> Cleaning up packages safe to remove

Uninstalling /usr/local/Cellar/mpv/0.9.2... (342 files, 35M)

Uninstalling /usr/local/Cellar/automake/1.15... (130 files, 3.2M)

Uninstalling /usr/local/Cellar/libass-ct/HEAD... (9 files, 440K)

Uninstalling /usr/local/Cellar/lua/5.2.4... (81 files, 1.1M)

Uninstalling /usr/local/Cellar/mpg123/1.22.2... (16 files, 656K)
```

## 参数

| Option | Description |
| --- | --- |
| `--force` | Overrides the dependency check for just the top-level formula you are trying to remove. If you try to remove 'ruby' for example, you most likely will not be able to do this because other fomulae specify this as a dependency. This option will let you remove 'ruby'. This will NOT bypass dependency checks for the formula's children. If 'ruby' depends on 'git', then 'git' will still not be removed. |
| `--ignore` | Ignore some dependencies from removal. This option must appear after the formulae to remove. |
| `--dry-run` | Does a dry-run. Goes through the whole process without actually removing anything. This gives you a chance to observe what packages would be removed and a chance to ignore them when you do it for real. |
| `--quiet` | No output |

## 提醒

软件作者给出了警告，要注意有些打包者没有正确地写好依赖。此外有些包可能此前被单独安装过，但后来又安装了依赖该包的软件，那么在卸载这个后安装的软件时也可能将之前单独安装的软件包一并卸载掉。

[原文链接](https://github.com/beeftornado/homebrew-rmtree)
