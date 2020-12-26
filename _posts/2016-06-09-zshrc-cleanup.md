---
layout: post
title: zshrc 整理
date: '2016-06-09 21:36:16'
yun: 佳
tags: 技术 TheSetup
---

之前是太懒——或者说是太勤快了，`zshrc` 文件基本上是每种机器一个，然后有一部分共用代码写到单独的文件里分别 `source`。最近因为配置本身出了点问题，这才真正下手把 `zshrc` 好好整理了一下。

我从 [oh my zsh](https://github.com/robbyrussell/oh-my-zsh) 获得大部分的 plugin：据说 [prezto](https://github.com/sorin-ionescu/prezto) 更快一些，但自从放弃 rvm 之后 oh my zsh 的性能也不再是瓶颈了。另外 oh my zsh 有个挺好用的选项是 `ZSH_CUSTOM`，指定它为 git repo 中的文件夹，这样就可以把来自 oh my zsh 的 plugin 和 theme 和自己的配置分开管理了。

所以思路大体上是所有机器共用一个 git repo 中的 `zshrc` 文件，symbolic link 到用户目录下面。具体系统相关的配置，比如 OS X/BSD/Linux 区别的；以及不同机器用途的配置，比如公司的机器上需要有 perforce 的配置，用作终端的机器上有 mosh 的配置，都写成 oh my zsh 的 plugin，放到 git repo 中，`ZSH_CUSTOM` 指定的目录。最后每个机器上 `touch` 一个标识类别的文件，在 `zshrc` 里用一个巨丑的 `if-elif-fi` 来指定各类机器的 plugins 列表。

最终 HOME 目录看起来是这个样子的：
```
# 标识这台机器是个人用途的 mac
#   pers-个人/serv-服务器/aka-公司
#   mac-Mac/lnx-Linux/bsd-BSD
.m_pers_mac
.omz # oh my zsh 的目录
.xxconfig # git blessed 个人配置文件
├── omz # export ZSH_CUSTOM=$HOME/.xxconfig/omz
│   ├── plugins # 'x_' 是为了区别 oh my zsh 官方的前缀
│   │   ├── x_emacs
│   │   ├── x_global
│   │   ├── x_keychain
│   │   ├── x_mosh
│   │   ├── x_osx
│   │   ├── x_p4
│   │   ├── x_svn
│   │   └── x_tmux
│   └── themes
└── zshrc
.zshrc -> .xxconfig/zshrc # symbolic link
```
