---
title: rm *.log 支持的最大文件数
layout: post
date: 2014-01-12 10:43:23
yun: 文
comments: true
tags: 技术 Linux
---

我倒是第一次遇到这个问题，在 `rm *.log` 大概有一万多个小文件的时候。

想了想觉得大概是 `shell` 的限制，Google 了一下，应该取决于这个 `_POSIX_ARG_MAX`，OS X 上默认是 4096.

可行的方案是用 `find` 一边找一边删除，像这样：

	find . -name '*.log' -print0 | xargs -0 rm
	


