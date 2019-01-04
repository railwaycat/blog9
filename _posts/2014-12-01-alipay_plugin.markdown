---
title: 支付宝 Mac 插件安装的文件们
layout: post
date: 2014-12-01 16:47:36
yun: 东
guid: urn:uuid:f85c5222-99d5-405e-a5ab-822dfa60d277
comments: true
tags: [alipay, macos]
---

大概是离国内的险恶互联网环境久了神经太放松，一不小心就着了支付宝插件的道。

即使是在 Mac OS X 上，支付宝依然要求必须安装一个插件才能从浏览器登录，这个插件是个 pkg 安装包，要求 root 权限。当时不假思索就装上了，回头想想才感觉有点不对劲。

打开 Activity Monitor 果然看到了一个 AlipayDispatcherService，系统启动即运行，不管 Safari 有没有打开，居然还 `kill` 不掉！`opensnoop` 看了一眼，感觉基本是在不停地读浏览器配置文件；`sample` 出来一看，果然是这样：有两个线程分别监视 Safari 和 Chrome 的扩展…

然后这货居然连个卸载程序都没有，真是让洁癖症者抓狂 ヽ(#`Д´)ﾉ

于是只好手动把安装包解开，看了一眼安装脚本，基本上被它丢进系统的文件如下：

1. `~/Library/Alipay` 及其下面的各种文件

1. `/Library/Application Support/Alipay` 及其下面的各种文件

1. `~/Library/LaunchAgents/com.alipay.refresher.plist`

1. `~/Library/Internet Plug-Ins/` 下面的 aliedit.plugin 和 npalicdo.plugin

1. `/Library/LaunchDaemons/com.alipay.DispatcherService.plist`

1. `/Library/Google/Chrome/NativeMessagingHosts/` 嗯，这个其实很恶心，明明我机器上完全没装 Chrome

以及两个开机即启动（确切说前者是登陆即启动，后者是开机）的服务 `com.alipay.refresher` 和 `com.alipay.DispatcherService`


	launchctl unload ~/Library/LaunchAgents/com.alipay.refresher.plist
	launchctl load ~/Library/LaunchAgents/com.alipay.refresher.plist 
	launchctl start com.alipay.refresher.agent

	sudo launchctl unload /Library/LaunchDaemons/com.alipay.DispatcherService.plist
	sudo launchctl load /Library/LaunchDaemons/com.alipay.DispatcherService.plist
	sudo launchctl start com.alipay.DispatcherService

如果只想停掉 AlipayDispatcherService 的话，用 `launchctl unload -w` 命令停掉这两个服务然后再 `kill` 应该就可以了；想卸载的话就只能手动把上面所有的文件删掉了。

网上有人提到它的 `refresher` 这个东西可能会检测删除和重新下载文件，不过我对 Mach-O 不是很熟，没法验证…总之下次坚决不能手贱，有任何可疑的东西都一定要在虚拟机里跑先 o(╥﹏╥)o

对了，以上分析都是基于版本 2.1.0.0，目前（2014-12-01）的最新版本。
