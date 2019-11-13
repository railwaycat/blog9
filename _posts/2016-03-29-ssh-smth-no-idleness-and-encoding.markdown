---
layout: post
title: SSH 水木的防发呆和编码
date: '2016-03-29 03:55:40'
yun: 艳
tags: linux
---

从 Terminal 下面 ssh 去水木站的话主要是两个问题，一个是编码，一个是防发呆。

编码问题我以前一般是在 screen 下面指定 `encoding gbk utf8` 来解决。

不过最近在看发呆问题的时候用了另一个方法，也可以处理：

```
#!/usr/bin/env expect

set timeout 60
encoding system gb2312
spawn ssh -1 <username>@bbs.newsmth.net
interact {
        timeout 30 { send " " }
}
```

其实两种方法在处理 term 界面上一些特殊字符的时候多少都有点乱码的问题，不过看文章的话能忍…
