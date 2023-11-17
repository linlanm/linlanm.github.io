---
title: 安装hexo-generator-archive插件
date: 2023-11-17 16:10:49
tags:
- Hexo
categories:
- Hexo
---

Archive generator for Hexo.

**Installation**
```text
$ npm install hexo-generator-archive --save
```
**Options**

```text
archive_generator:
  enabled: true
  per_page: 10
  yearly: true
  monthly: true
  daily: false
  order_by: -date
```
enabled: The default value is true, set to false if you do not want to enable the plugin
per_page: Posts displayed per page. (0 = disable pagination)
yearly: Generate yearly archive.
monthly: Generate monthly archive.
daily: Generate daily archive.
order_by: Posts order. (Order by date descending by default)
**License**
MIT
