---
title: 更新版本报external_link解决办法
top: 0
categories:
  - hexo
  - null
essayending: true
declare: true
valineenbale: true
toc: false
reward: true
abbrlink: 6a79fa5d
date: 2021-11-13 16:43:27
tags: [随笔]
---
在``hexo``到新版本，发现升级后运行``hexo s``的时候会出现如下的报错情况：
![](  https://cdn.jsdelivr.net/gh/liuliange/blogPicture@main/blogPicture/20211113165449.png)

由于新版的``HEXO``增加了不少新特性，因此需要修改默认的配置模版文件。
<!-- more -->
解决方法:打开主目录下的_config.yml文件，将如下内容做调整。
``external_link: true # Open external links in new tab``
将上面的内容，修改为如下内容。

`` external_link:
  enable: true # Open external links in new tab
  field: site # Apply to the whole site
  exclude: '' ``

接着重新运行 ``HEXO``命令，一切正常。
