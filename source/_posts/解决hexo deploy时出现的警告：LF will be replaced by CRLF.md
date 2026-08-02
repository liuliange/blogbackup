---
title: 解决hexo deploy时出现的警告：LF will be replaced by CRLF
top: 0
categories:
  - hexo
  - null
essayending: true
declare: true
valineenbale: true
toc: false
reward: true
tags: [hexo, 随笔]
abbrlink: 589ff110
date: 2022-09-26 15:20:26
---
Windows下在使用hexo d命令部署博客时，会出现下面这个警告：
```
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in ****.html.
```
 <!-- more -->

原因：windows下换行符为CRLF，Linux下换行符为LF（使用Git命令行Git Bash，实际上就是相当于在linux环境），而git工作区默认换行符为CRLF，当执行git add ... 时就会出现警告！当最终push到远程仓库时git会统一格式全部转化为用CRLF作为换行符，故不必对其做转换操作，文本文件保持原样，只需执行以下命令即可。

```
git config --global core.autocrlf false
```

参考资料：
https://stackoverflow.com/questions/17628305/windows-git-warning-lf-will-be-replaced-by-crlf-is-that-warning-tail-backwar