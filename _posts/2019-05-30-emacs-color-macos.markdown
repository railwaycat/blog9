---
title: Emacs 配色 macOS 篇
layout: post
date: 2019-05-30 12:41:07 -0400
yun: 卅
comments: true
tags: emacs
---

接[上篇](/2016/12/08/emacs-color-tips-under-terminal.html)。

## 一
相对于暗色主题，我更偏好亮色和暖色一些，之前用了很长时间的自制类 Terminal.app 带的 Novel 主题，后来因为色彩支持的关系，又用了一段时间基于 [Purcell](https://github.com/purcell/color-theme-sanityinc-solarized)，自己小修补之后的 [Solarized Light](https://github.com/railwaycat/color-theme-sanityinc-solarized)。有一天我忽然意识到一个问题，既然现有的软件和硬件系统已经支持了自动亮度调节、Night Shift 还有 True Tone，可以根据时间，周围的亮度和光照冷暖自动调节屏幕的显示，那亮色主题里再主动去用暖色和冷色的背景色还有意义么？换句话说，如果简单用白底黑字，而把冷暖色效果完全交给系统去自动完成是不是更好呢？

所以我回到了系统默认的 Basic Terminal 主题，Emacs 也是默认的白底黑字，只是按照自己的喜好稍微修饰了一下 mode line，和 company mode 这类默认色彩实在是太突兀的 mode。

效果…目前还好，终端、Emacs、浏览器都保持白底黑字的话，我可以把屏幕亮度保持在一个很低的水平上，而依然可以保证辨识度和眼睛的舒适。长期的话，容我再用用看。

## 二
macOS 从 10.14 Mojave 开始加入了系统级别亮色和暗色的切换，Emacs mac port 也从 mac-7.2 开始支持这个特性，而且更进一步——它加入了一套新的色彩格式：`mac:COLOR-LIST-NAME:COLOR-NAME`。函数 `mac-color-list-alist` 可以列出所有支持的 color-list-name 和 color-name。

`System` list 下面的各个颜色是跟着系统设置的变化走的，比如 `mac:system:textColor` 在亮色的时候是黑色，暗色的时候就是白色；`mac:system:selectedTextBackgroundColor` 就是系统设置里 General->"Highlight color" 的设置。

## 三
设置部分反而是最简单的，原理在上面已经提到了。我自己用的主题也只是三五行的简单修饰。颜色的话，`(eq window-system 'mac)` 可以判断是不是 mac port，如果不是的话，就直接用 256 color里的颜色，这样终端和 Emacs 窗口也可以保持一致。NS port 似乎到目前为止还没有类似的支持。

我把它放在 [gist](https://gist.github.com/railwaycat/39df7d1ad3cc5214423799dcfb1954c4) 上了。有心的话，按照自己的喜好打磨出一个能自动适配系统亮色或者暗色的主题应该不难。


