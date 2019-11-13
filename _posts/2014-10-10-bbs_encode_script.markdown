---
title: BBS 转码脚本
layout: post
date: 2014-10-10 15:04:43
yun: 灰
guid: urn:uuid:5e03fe71-7354-4e5b-b6f1-6aca8469bfde
comments: true
tags: linux
---

自从把主力连接变成 mosh 之后，随时合上笔记本然后到处乱跑就成了习惯。

于是现在唯一剩下的例外就是 BBS 了。

大体上 BBS 是基于 telnet 或者 ssh 协议，理论上任何一个 telnet/ssh 客户端都是可以用来登陆的，不过基本上所有的 BBS 都充斥了乱七八糟的特殊字符和各种色块儿。传统上的 term 客户端基本是都有做转码，而且也为 BBS 上的特殊字符（图案和色块儿）做了优化，直接 telnet/ssh 多多少少都会有点显示问题。

当然最主要的问题还是编码。

BBS 的历史问题，使得不管是水木还是 ptt 还是其他一干小的 BBS，无一例外都是本地编码。大陆的主要是 GB2312，台湾的是 big5；UTF-8 的目前是一个都没见到。而现在不管是各种 Linux 发行版还是 Mac，locale 都是 UTF-8。虚拟终端的默认编码一般也一样。

觉得比较好的解决方法就是写个 telnet/ssh 的 wrapper 来转码了。动手之前先 google 了一把，发现以前也有人有过跟我一样的想法，看了一下东西写得挺好，就直接拿来了……

[http://pt0079.blogspot.com/2011/05/gnu-screen-encodingbbs.html](http://pt0079.blogspot.com/2011/05/gnu-screen-encodingbbs.html)

脚本是给 SEU BBS 写的，但试了试至少水木和 PTT 都是正常的。  
以后 VPS 上挂的除了 IRC 还有 BBS 了（￣へ￣）

update: 登陆的时候务必使窗口大于 84x25
