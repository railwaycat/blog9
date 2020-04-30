---
layout: post
title: Emacs tramp hangs when reconnecting
date: '2016-02-22 21:11:41'
yun: 养
tags: 技术 Emacs
---

之前大概有接近两年时间都是从 Mac 上 mosh + tmux 到远程 Linux box 然后在终端里用 Emacs；最近开始重拾 Mac 上的 Emacs 和 tramp。没有什么特别的动力驱使，原因的话一是想多用用 Emacs mac port，自己参与维护的自己都不用多少有点说不过去，二是找点新鲜感，另外能显示斜体[^1]和图片看上去也好看点不是么。

不过用久了 mosh 养成了连接一直保留、随手合笔记本的习惯。基本上 tramp 是支持这种模式的：tramp 维持一个 `ControlMaster=auto` 的 ssh 连接，然后平时的文件操作都会共用这个连接；如果连接失效了，tramp 会自动重连。

只是实际中用的时候偶尔会遇到 tramp 重连的时候卡在那儿，然后直接卡死整个 Emacs 的情况，而理论上能一劳永逸解决卡死问题的 Guile Emacs 现在还没影[^2]…

解决的方法有两个：

一个是长按 `C-g` 直到 tramp 退出连接状态，然后 `M-x tramp-cleanup-all-connections`

另外一个简单粗暴的方法是新开一个终端，找到 tramp 的 ssh 会话然后杀掉…

这个会话的命令一般长这样：

```
ssh -o ControlMaster=auto -o ControlPath=tramp.%r@%h:%p -o ControlPersist=no -e none

```

[^1]: iTerm 加上一些 terminfo 也可以在终端下显示斜体，不过我没尝试过，也不确定Emacs 的支持程度。
[^2]: 我搞错了，Guile Emacs 也是单线程的T_T
