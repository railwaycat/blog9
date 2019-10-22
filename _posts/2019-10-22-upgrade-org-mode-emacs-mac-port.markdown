---
title: Emacs mac port 升级 org-mode
layout: post
date: 2019-10-22 14:46:22 -0400
yun: 养
comments: true
tags: emacs
---

Emacs 自带 org-mode——只是版本会落后 org-mode 的最新版本一段时间。所以很多人会选择从 elpa 或者是 org 的源安装最新的版本。

不过 Emacs mac port 对自带的 org-mode 做了一些修改，让它能支持全局的 org-protocol 还有 Retina 显示屏下对图像显示的优化。如果选择另外安装 org-mode 的话，会遇到一些显示问题，以及 org-capture 的时候提示 `"Symbol’s function definition is void: server-edit"` 等错误。

如下的补丁可以解决这些问题：

<https://gist.github.com/railwaycat/26f195feba1b4ce5df05e4bd01eb1d44>

这个补丁是从 Emacs mac port release 中提取的，并适配了 org-9.2.6。如果是用在 elpa 下安装好的 org 目录的话，记得要 byte-compile 一下这三个文件

    org.el
    org-protocol.el
    org-compat.el
