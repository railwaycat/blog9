---
title: 个人的 ssh key 策略
layout: post
date: 2017-02-24 10:57:55
yun: 迥
comments: true
tags: 技术
---
## 目前的策略
目前的策略是每台机器有自己的 ssh key，ssh key 不离开机器。这样的话整体上安全性是可以保证的，出了什么问题也可以单独 revoke 某台机器的访问。缺点的话是折腾`(._.)`：

- 一个是管理起来麻烦，像机器之间交叉访问的时候，每台机器上都会有很长的一串预置的 `authorized_key`，而且自己机器的 pub key 跟其他机器的混在一起，不方便管理。还有 Github 私有库这种每台机器都有可能访问的东西，上面的 pub key 列表也是一长串。
- 二是因为太折腾所以懒得去按计划更新……原计划是每年手动更新，但随着机器数量和交叉访问的变多，时间最久的 key 已经快两年了。

## 新的策略
目前的想法是借鉴我司的处理方式，按照用途划分而不是机器，把 ssh key 分成高中低三个安全级别。家里不对公网的台式机，随身的笔记本是高等级（偷了个懒，全硬盘加密的笔记本视作是即使失窃也算安全的）；VPS 和家里对公网的机器是中等级；针对特定任务（比如远程备份）的 key 是低安全。更新时间也是视安全等级递减。

核心策略只有一条： 某一个等级的 pub key 只会存在于同等级或者更低等级的机器 `authorized_key` 文件里。

## 结
新的策略目前没想到有什么大的漏洞，先把之前的 ssh key 都 revoke 掉，按照新策略实行几天看看…