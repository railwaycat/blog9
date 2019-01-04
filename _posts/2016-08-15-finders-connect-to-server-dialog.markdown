---
layout: post
title: Finder 的连接服务器对话框
date: '2016-08-15 20:04:27'
yun: '删'
tags:
- macos
- applescript
---

作为一个非重度的 VNC 和 SMB 用户，macOS 的 Finder 内置的 VNC 客户端和 SMB 连接已经很够用了。唯一的缺点是每次呼出都得先让 Finder 窗口先 active 然后再点菜单或者 `Cmd-k`。

可惜 Finder 并没有直接可以操作这个对话框的 AppleScript 选项，所以只能用模拟菜单点击或者按键的方式。代码如下，它可以 active Finder 然后呼出 `Connect to Server...` 对话框。把它加入 Alfred 或者设置一个全局的快捷键悉听尊便。

```
tell application "Finder" to activate
tell application "System Events"
    tell process "Finder"
        keystroke "k" using command down
    end tell
end tell
```