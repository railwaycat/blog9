---
layout: post
title: Alfred/Applescript 管理和切换 Google Chrome 帐号
date: '2016-08-10 01:29:08'
yun: '灰'
tags:
- macos
- chrome
- applescript
---

因为 Safari 的书签功能太傻b所以转用 Google Chrome。感觉 Safari 完全没有把书签当成一个第一梯队的功能：当前页面加入书签至少要点击两次；书签不支持排序；同名书签还可以重复…书签数量上去之后实在是忍无可忍。

但 Chrome 我平时也用，主要是工作上用，然后关联着公司帐号的 Google Apps。这样加上个人的帐号之后就面临着帐号切换的问题。作为默认浏览器的话，Chrome 会用最后一个激活的窗口的帐号来打开新链接，创建匿名浏览窗口也是同理。这导致我如果不想把工作和个人的浏览记录混在一起的话就一定得有个能方便切换帐号的工具才行。

Chrome 有命令行参数可以指定 profile[^1]，不过只能用在第一次启动的时候。可惜 Mac 上 Chrome 并没有提供 Applescript 的接口来操作 Profile，所以只有用 Applescript 模拟鼠标点击这一条路了。但这两个操作的别扭点在于，鼠标模拟点击的是菜单`People...`下的用户名，而命令行参数接受的是 Profile name，Profile name 在`~/Library/Application Support/Google/Chrome/`下可以找到，通常是`Default`，`Profile 1`这样的存在。

所以如果只是简单 hack 的话，Applescript 很难写得通用。这也是我不打算把它做成 Alfred Workflow 放到 Github 上的原因。用法的话可以 Alfred 里面新创建一个 keyword 到 Applescript 的 workflow，然后自己选择合适的 keyword 和参数，把下面这段代码贴进`Run NSAppleScript`就行，当然还需仔细改一改 hardcode 在里面的用户名和 Profile name。


```
set args to (q as string)
if application "Google Chrome" is running then
	if args is "x" then
		set profileName to "Xin"
	else if args is "a" then
		set profileName to "Work-Xin"
	else
		set profileName to "Xin"
	end if
	
	tell application "System Events"
		tell process "Google Chrome"
			click menu item profileName of menu "People" of menu bar item "People" of menu bar 1
		end tell
	end tell
else
	if args is "x" then
		set profileDir to "Default"
	else if args is "a" then
		set profileDir to "Profile\\ 1"
	else
		set profileDir to "Defualt"
	end if
	
	do shell script "open -a /Applications/Google\\ Chrome.app --args --profile-directory=" & profileDir
end if

tell application "Google Chrome" to activate
```


[^1]: http://peter.sh/experiments/chromium-command-line-switches/