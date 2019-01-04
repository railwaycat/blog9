---
layout: post
title:  "info files for Homebrew"
date: 2014-01-05 23:20
yun: 微
guid: urn:uuid:efa5532d-62d8-4d7d-a869-7a734b2dd3d6
comments: true
tags: [homebrew, macos]
---

Homebrew 默认是不安装各个 formula 的 info 文件的，需要安装的时候需要这样一下：

    export HOMEBREW_KEEP_INFO=1

按照 man brew 的说法，之后如果需要查看这些 info 的时候，还得确保 `/usr/local/share/info` 在 `INFOPATH` 里：

    export INFOPATH="/usr/local/share/info:$INFOPATH"

虽然对于 Emacs 来说不需要设置也能访问就是了（因为 Emacs 的 info path 是 Emacs 自己的环境变量）…

我也是翻了很久才从 `man brew` 里翻到的，一开始还以为是自己脚本的问题+_+

