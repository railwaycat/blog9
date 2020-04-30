---
layout: post
title: Gcc preprocessor output debugging hint
date: '2016-02-16 16:08:10'
yun: 铣
tags: 技术 C++
---

官方文档： https://gcc.gnu.org/onlinedocs/cpp/Preprocessor-Output.html

首先用 `-save-temps` 开关保留编译的中间文件。`.ii` 文件是预处理的输出。

对于像这样的 debugging 提示行
```
# linenum filename flags
// example
# 1 "/usr/include/c++/4.6/x86_64-linux-gnu/./bits/c++config.h" 1 3
```

其中 flags：

1. 开始新文件
2. 结束 include，退回上层文件
3. 表示当前文件来自系统头文件—— `#include <>`
4. 表示当前文件是包含在 `extern "C"` 块里的。

上面的例子就读作“源文件第一行 include 的 `c++config.h` 文件作为系统头文件的 include 起始点”。
