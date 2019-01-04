---
title: /usr/local/ 之外的 Homebrew
layout: post
date: 2014-01-12 20:20:20
yun: 文
guid: urn:uuid:294c797a-0229-4be7-af58-78da301b2a5c
comments: true
tags: [homebrew]
---
我比较喜欢 Homebrew 的一点是它对非自己系统里的包能稍微宽容一点，不像 Macports，我至今没找到让 Macports 认识 MacTeX 的方法[^ft]… 但我没法忍受 Homebrew 自作聪明的占领 `/usr/local/` 还修改权限。`/usr/local/` 跟贫乳一样是稀缺资源，我还是倾向于把它留给自己修改和编译的一些包。

于是我选择了把它移出去，移到 `/opt/homebrew`。

安装过程很简单，先

    cd /opt
	sudo mkdir homebrew
	
接着改成自己想要的权限，我的策略是 owner 是用户，组还保持 wheel。

    sudo chown `whoami` homebrew

然后

	git clone https://github.com/Homebrew/homebrew.git
	
最后修改下 `$PATH` ，把 `/opt/homebrew/bin` 加进去就完事了。

值得一提的是如果安装在非 `/usr/local/` 目录下的话就没法用预编译的 bottles 了。可以把这个变量加上来免去每次安装 formulae 时的罗嗦。

	export HOMEBREW_BUILD_FROM_SOURCE=1

就是这样。

---

[^ft]: 有一些包有 `mactex` 选项，但我从没见它正常工作过。