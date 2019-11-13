---
title: Dynamic DNS records on Google Domains
layout: post
date: 2018-01-07 15:48:17
yun: 虞
comments: true
tags: tech_notes
---

家里有一台机器挂在路由器的后面，通过一个 port forwarding 曝露在公网上。美国的 ISP 运营商其他的不便评说，但至少给公网 IP 地址足够慷慨，虽然是 DHCP 的家用宽带，
但一般 lease 到的 IP 地址能持续几个月乃至大半年。

所以我很长一段时间的用法都是用一个脚本每隔半天往 VPS 上的一个文件写一下自己的 IP
地址，有变化的时候手动把新的 IP 地址更新到各个机器的 ssh config 文件里去。这样也
不是不好，但总感觉有点不够自动。

最近手上有了空闲的域名，开始想点这方面的心思了`ˊ_>ˋ`

Google Domains 自带了 Dynamic DNS 的支持，只是不管是选项还是文档都藏的有点深，这
里简单记录一下，免得下次再用的时候自己也忘了。

- 配置在 My Domains -> &lt;domainname.tld&gt; -> DNS，点进去之后在 Synthetic records 一
  栏下面，点 "Subdomain forward" 下拉菜单里有 Dynamic DNS 的选项。

- 文档在这里：<https://support.google.com/domains/answer/6147083>

我自己配置的时候用了 ddclient[^1]，跟着 Google Domains 网站和配置文件里的例子走，非常简单。

---
[^1]: https://sourceforge.net/p/ddclient/wiki/Home/
