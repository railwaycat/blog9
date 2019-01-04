---
title: using mosh with tmux
layout: post
date: 2014-07-21 15:15:05
yun: 马
guid: urn:uuid:b5d9dee6-fe1c-4420-a2b2-5272e79e4af6
comments: true
tags: [mosh, tmux]
---

办公室的无线网络实在是太差，只要一有客户过来，网络里用户激增的时候整个无线就会每隔十几分钟掉一次，于是只好重拾 [Mosh](http://mosh.mit.edu/)。

Mosh 现在最大的缺点在于没有历史，不能滚动回去看之前的输出，这个基本是致命的。我在[之前](/2014/05/13/mosh.html)也提到过了。可惜的是到目前这个 feature 依然还是仅仅停留在纸面上，没有任何动工的迹象( ＿ ＿)ノ｜

搭配 tmux 用就基本是唯一一个途径了。

写了两个脚本来方便每次 attach 和新建 session。我常用的两个机器是跑 Fedora 的 `fedbox` 和私人的 `atago`，所以简单加进了个单字母的别名。fedbox 和 atago 是定义在 ssh config 文件里的主机。

两个脚本重复的地方其实不少，不过觉得没必要为了好看而增加复杂度…

update: 把这两个脚本写成了 zsh 的函数，另外还加了个显示远端 session 列表的。

update again: 后来又系统地更新了一次: [](/2016/06/09/zshrc-cleanup.html)