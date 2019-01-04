---
title: 组内的 IDE 使用统计
layout: post
date: 2018-08-11 15:08:51 -0400
yun: 直
comments: true
tags: the_setup
---

前几天组内有个开发工具的使用统计，不清楚原意，可能是公司想知道需要购买的商业软件授权的数量吧，但结果还是挺有意思的。

我们组一共十个人，manager 自己还有另外一个人没有回复邮件，所以一共统计了八个人的。

公司给我们的开发工具配置是一个笔记本，MacBook 和 Thinkpad 之间选一个，外加一个 Linux 的台式机。开发环境是在 Linux 上的 chroot 容器，就我所知道的，直接在 Linux 上写代码开发的和在笔记本上写代码然后 rsync 到 Linux 机器上编译的大体上对半。

开发内容的话，主要是 C++，另外有一部分代码是 C 写的，还有一小部分 Perl 的胶水代码。

有三个人的主力开发工具是 Eclipse CDT，很是让我惊讶。我一直以为 Eclipse 已经淡出主流开发工具的行列了。另外其中有一个人使用 Emacs，具体说是 eshell 配合 grep/grep-find 搜索代码…而且仅仅用来搜索代码`(°_°)`

使用 Emacs 的有四个，都是配合 gtags。不过其中有两个表示也会用其他的工具浏览代码，分别是 Eclipse CDT 和用 wine 跑在 Linux 下的 Source Insight。另外一个人拿着组里唯一一台 Thinkpad，表示日常开发在 Windows 下 VMWare Station 里的 Linux 虚拟机里…剩下那个是个原教旨的 Emacs + gtags 用户。

最后剩下的那位是个 Vim + ctags/cscope 用户，他常年在 Linux 的~~终端（不是终端虚拟器）~~控制台字符界面下干活。

就是这样了。
