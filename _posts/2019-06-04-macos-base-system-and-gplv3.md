---
title: macOS base system and GPLv3
layout: post
date: 2019-06-04 11:45:13 -0400
yun: 支
comments: true
tags: 技术 Mac
---

留意到 macOS 10.15 Catalina beta 的 [release notes](https://developer.apple.com/documentation/macos_release_notes/macos_10_15_beta_release_notes/) 里有这样一段话：

> Scripting language runtimes such as Python, Ruby, and Perl are included in macOS for compatibility with legacy software. Future versions of macOS won’t include scripting language runtimes by default, and might require you to install additional packages. If your software depends on scripting languages, it’s recommended that you bundle the runtime within the app.

另外从 beta 里也可以观察到：

1. Emacs 从系统里被移除

2. 系统的默认 shell 改成了 zsh[^1]

从 GPLv3 开始，跟所有的 BSD 血亲一样，macOS 一直很小心的保持自己与 GPLv3 完全隔离。受制于此，不少原本系统自带的软件都无法得到更新，而一直保持在 GPLv2 的老版本，比如停留在 22 的 emacs 还有停留在 3.2 的 bash。

之前一直好奇 Apple 要怎么解决这个问题，因为随着时间的推移，这部分老版本的软件会逐渐不再被维护也不会再被依赖。macOS 的用户迟早越来越多遇到这样的场景：虽然某软件/库系统已经带了，但实际中还需要用 macports 或者 homebrew 来再装一份以保持其他软件的依赖需求。

从 Catalina 的举动来看，Apple 的策略是大幅度精简 macOS，一直精简到类似 FreeBSD 的 base system，只保留系统必需和最基本的组件，其他的部分由第三方包管理器提供或者由 bundle 中自带。同时 SIP/rootless 的存在也可以大幅度减少用户搞坏系统的几率，反过来，精简化的 base system 也可以减少 SIP 给用户带来的麻烦，因为被写保护的部分越来越核心也越来越少。

对于 base system 来说，无 GUI 支持的老版本 Emacs 属于可以被剔除的部分；bash 属于系统兼容性必需，必须留着，但 3.2 版本对于保持兼容性来说已经足够，让用户默认使用得到最新更新的 zsh 是更佳选择。Release notes 里面提到的 Python/Ruby/Perl 虽然不属于 GPLv3 隔离的部分，但想必会被严格划分成「满足系统需要的」和「满足用户需要的」，前者应该会在 `/System/Library/Frameworks/` 下面藏得更严实，pip/gem 的功能也会被限制，后者就如 release notes 所说要用户自己装了。

是条令人满意的路，虽然动作得慢了一点。

[^1]: <https://support.apple.com/en-us/HT208050>
