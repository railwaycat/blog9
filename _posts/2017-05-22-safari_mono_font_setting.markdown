---
title: Safari 的默认等宽字体设置
layout: post
date: 2017-05-22 11:33:31
yun: 养
comments: true
tags: macos
---

我也不知道 Safari 到底是出于什么考虑……

最早的时候 Safari 是有设置默认字体的地方的，后来没了。

然后的时候 Safari 是可以通过自定义 CSS 设置默认字体的，后来不支持了。

现在只剩下一种方式了，希望能存在得久一点：

```
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2FixedFontFamily TheSansMonoCd
```
