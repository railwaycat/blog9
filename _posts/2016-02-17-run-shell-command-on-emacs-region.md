---
layout: post
title: Run shell command in Emacs
date: '2016-02-17 17:28:02'
yun: 篠
tags: 技术 Emacs
---

两个快捷键：

```
M-! # 在 Emacs 中运行外部指令
M-| # 在 Emacs 中运行外部指令作用在当前选中的区域上
```

这两个命令的默认输出都是 `*Shell Command Buffer*`，如果想让它们输出在当前的 buffer 光标所在的位置的话可以加上 `C-u` 前缀。

话说这两个命令我一般是按 `Esc` 再按对应的符号，不知道有没有其他更简便的方法——不用脚踏板的话。
