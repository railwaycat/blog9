---
title: 在 Terminal/iTerm2 和 tmux 中使用英文斜体
layout: post
date: 2017-05-05 14:11:30
yun: 微
comments: true
tags: 技术 Mac
---

# 0
iTerm2 和 Sierra 上的 Terminal.app 都已经支持在终端里显示斜体字形，这在使用 Emacs 之类的编辑器的时候格外有用一点。

# 1. (skip if you are not a fan of history)
虚拟终端，或者叫终端仿真器（Terminal emulator）的斜体字形支持本身是个有点 tricky 的事情。

从终端类型和仿真器说起。

上溯历史的话，早期很长一段时间里，终端（Terminal）意味着摆在用户面前真真正正的硬件，由一个显示器一个键盘一点点内存和通信接口（也许还有鼠标）组成的硬件。而终端的显示功能也不外乎（对应 ASCII 表，仅考虑英文字母）：

1. 支持ASCII 可显示字符的输出。
2. 支持控制字符和 Escape sequence 来完成状态的控制和复杂的显示。

第一个是一一对应且没有异议的；但第二个，当时不同的厂家出厂无数不同型号的终端，但自始自终并没有形成一个统一的标准，所以主机系统为了支持不同类型的终端，只能在系统里维护一个囊括所有支持的终端类型的数据库，其中记录了支持的每类终端对控制字符和 escape sequence 的支持和定义。这种数据库现在最常见的是 terminfo[^1]。

硬件终端里，DEC 的 VT102、VT220 是当时相当常见的终端型号；而随着硬件终端基本退出现实应用，终端仿真器自己也变成通行标准的一部分，比如 xterm、 xterm-256color、rxvt，还有 GNU screen 作为 terminal multiplexer 的标杆， screen 及 screen-256color 等衍生类型也被 terminfo 收录。

回到讨论的起点，所以说如果想要能显示斜体字形的话，下面几个条件缺一不可：

1. 终端仿真器程序对斜体的支持
2. （如果使用 tmux 的话）tmux 作为跑在仿真器上的复用器（multiplexer）对斜体的支持
2. 终端仿真器程序和 tmux 支持的终端类型有斜体的 escape sequence 的描述

# 2. iTerm/Terminal.app
iTerm 和 Sierra 上的 Terminal.app 已经有了对斜体字的支持，但 macOS 自带的 terminfo 里，xterm* 族的终端类型并没有对应的更新。

所以需要通过下面这条命令来重新生成一份 `xterm-256color` 的 terminfo 文件，放在用户目录下面，override 系统自带的 `xterm-256color` 类型。因为本身是利用加载的优先级去覆盖系统自带的部分，所以只准备 override 最常用的 `xterm-256color`，如果常用其他终端类型的话也可以稍作改动。

1. 把下面的内容保存进一个文件，比如文件名 `xterm_256color.terminfo`

    ```
    xterm-256color|xterm with 256 colors and italic,
      sitm=\E[3m, ritm=\E[23m,
      use=xterm-256color,
    ```

    这些内容的意思是继承当前的 `xterm-256color` 类型，并且增加`sitm` - "enter italics mode" 和 `ritm` - "exit italics mode" 两个escape sequence 的值。

2. 执行：`tic -x xterm_256color.terminfo` 来生成最终的`xterm-256color` 文件并自动放置到用户目录 `~/.terminfo/` 下面。

命令执行成功之后的话，在 `~/.terminfo/78/` 或者 `~/.terminfo/x/` 目录下面应该可以找到文件名为 `xterm-256color` 的文件了。

重启 iTerm/Terminal.app 之后应该就可以看到斜体字形了。  
![](/content/images/2017/05/iterm_italic.png)

另外 `infocmp xterm-256color` 命令也可以验证当前终端使用的是哪一个文件。

# 3. tmux
tmux 要比终端仿真器要稍微复杂那么一点点。

所谓后发优势，tmux 在最早开发的时候就已经看到了终端类型的乱象，于是在FQA[^2] 里直接声明，tmux 仅支持以 screen 开始的一族专门为终端复用器定义的终端类型。这个强力约束在很长时间内规避了无数的诡异显示问题，但终于碰上了这个——斜体显示。

GNU screen 本身并不支持斜体，所以 screen* 一类的终端类型自然也没有显示斜体字形的描述（更准确的说，有一部分但描述不正确[^3]）。

在 v2.1 之前，tmux 的态度是听之任之，用户可以通过跟上面差不多的方式自己 override 一个支持斜体字形显示的 `screen-256color` 来。但从 v2.1 之后，tmux 采用了一个更符合标准的做法：为 tmux 的斜体字型还有未来可能的更多 feature 而在 terminfo 里定义了官方的 `tmux` 和 `tmux-256color`。而如果终端类型被指定为 `screen` 一族的话则**完全**不显示斜体。根据官方文档，最新的系统里应该已经陆续包含这两种类型，不是很清楚这里的“最新”到底有多新但总之我手头的所有系统里都还没有见到…

在最新的终端类型出现在所有系统之前，补救措施是用跟上面类似的方法手工生成这两个文件。tmux FAQ 里推荐的方法是这条命令：

```
$ cat <<EOF|tic -x -
	tmux|tmux terminal multiplexer,
		ritm=\E[23m, rmso=\E[27m, sitm=\E[3m, smso=\E[7m, Ms@,
		use=xterm+tmux, use=screen,

	tmux-256color|tmux with 256 colors,
		use=xterm+256setaf, use=tmux,
EOF
```

在大部分 linux 上面都没有问题，但在 macOS 上，即使是最新的 Sierra 上也会因为 `tic` 的版本不够新而出错。可以把它稍微调整一下：

1. 把下面的内容保存进一个文件，比如文件名 `tmux.terminfo`

    ```
    tmux|tmux terminal multiplexer,
      ritm=\E[23m, rmso=\E[27m, sitm=\E[3m, smso=\E[7m, Ms@,
      use=xterm, use=screen,

    tmux-256color|tmux with 256 colors,
      use=xterm-256color, use=tmux,
    ```

    参考之前 `xterm-256color` 的定义，这些内容定义了支持斜体字形的两条escape sequence `sitm` 和 `ritm` 的值，并且修正了不正确的 `rmso` - "exit standout mode" 和 `smso` - "enter standout mode" 的值。

2. 执行：`tic -x tmux.terminfo` 来生成最终的 `tmux` 和 `tmux-256color`文件并自动放置到用户目录 `~/.terminfo/` 下面。

命令执行成功之后的话，在 `~/.terminfo/74/` 或者 `~/.terminfo/t/` 目录下面应该可以找到文件名为 `tmux` 和 `tmux-256color` 的文件了。`infocmp tmux-256color` 命令也可以验证。

最后修改一下 tmux 的配置文件，增加或者修改原有的这一行：

```
set -g default-terminal "tmux-256color"
```

重启 tmux （切记重启整个 tmux 进程，而不是某个 session），之后应该就可以看到斜体字形了。  
![](/content/images/2017/05/iterm_tmux_italic.png)

# 4. 一些坑
## Emacs
当前的 Emacs 版本 25.2 并不支持 `tmux` 一族的终端类型，所以在终端下运行的时候会有错误提示，不过更新的开发中的 26 已经加进了 `tmux.el` ，所以解决方案的话是要么从最新的 trunk code 编译一份，要么手工把 trunk 下面的 `lisp/term/tmux.el` 文件放到当前版本对应的位置： 大约是 `/usr/local/share/emacs/25.2/lisp/term` 或者 `Emacs.app/Contents/Resources/lisp/term/`，取决于 Emacs 安装的方式。

## 不便于改动的系统
生产环境总有些不可以随便改动的系统，这样从 tmux 里 ssh 过去的时候，ssh会继承当前会话的 `TERM` 值，即 `tmux` 或者 `tmux-256color`，但 less, vi 等一大票程序都会报错说不认识当前的终端类型。我现在用的解决方法是另外定义一个指定终端类型的别名（忽略我随心所欲的命名）：

```
alias sah='TERM=xterm-256color ssh'
```
# 5. Fin

[^1]: terminfo 是更早的 termcap 库的升级和替代。
[^2]: https://raw.githubusercontent.com/tmux/tmux/master/FAQ
[^3]: https://github.com/tmux/tmux/issues/175
