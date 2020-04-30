---
layout: post
title: Emacs 在终端下的配色技巧
date: '2016-12-08 21:52:52'
yun: '齐'
tags: 技术 Emacs
---

其实也不是什么神奇的技巧(´▽`)ﾉ

一般终端的色彩主题都会有一个 16 色的调色盘，分别是

>black, red, green, yellow, blue, magenta, cyan, white

和它们各自对应的 bright 版本
> brightblack, brightred, brightgreen, brightyellow, brightblue, brightmagenta, brightcyan, brightwhite

还有前景色和背景色。

这些色彩 Emacs 可以在定制主题的时候用名字直接引用，另外前景色是`unspecified-fg`、背景色是 `unspecified-bg`。

Emacs 支持的色彩远比一般的终端模拟器要多——最常见的是 16 色和 256 色的终端。所以如果某些色彩主题用上了超出 256 色的颜色的话，可以策略地把这些颜色定义到终端色彩主题的调色盘里，然后通过调色盘引用。

就是这样。
