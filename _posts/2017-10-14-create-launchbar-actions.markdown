---
title: LaunchBar Actions 动手写
layout: post
date: 2017-10-14 21:08:29
yun: 寒
comments: true
tags: mac
---

# #0
试用了一下 LaunchBar 6，发现它已经支持中文了：虽然还不能用中文作为指令，
但是可以在按空格之后的输入框里输入中文。作为启动器的话，这种程度的中文
支持已经可以让我满意了。

但尝试把 Alfred 上面自己写的 workflow 往 LaunchBar 上面迁移的时候才发
现 LaunchBar 的 action 社区实在是…荒芜，官方论坛里十年不过积累了十页帖
子，其中还有小半是上来求助问题的，官方的文档[^1]也是惜字如金，连
Google 上都没搜到很多 action 开发相关的文章，不管是中文还是英文。

摸索着把自己常用的一个新开启终端窗口的 workflow 写成了 LaunchBar 的
action，顺手把过程记录在下面，也是给其他人（还有一段时间后肯定会忘光的
自己）留个路标吧。不过我并不打算把它写成一个 step-by-step 的教程，毕竟
Action Editor 的操作挺直观的，官方文档页不算完全无用，我只打算记录下我
踩的坑，和文档中含糊不清的地方。

# #1
我自己在 Alfred 下常用的这个 workflow 是这样的：在 iTerm 里我有一套的
profile，对应日常不同的场景，然后每种分别有浅色和深色各一套。分别是：
Light/Dark/Light code/Dark code/Light read/Dark read。因为这是日常中几
乎最常用到的操作，我需要一个简洁且快速的方式来打开我需要的终端窗口。

workflow 在不带参数的情况下用默认的 profile 打开一个新窗口，带参数的情
况用以参数为名字的 profile 打开新窗口。另外在提示的时候可以把待选择的
profile 的名字显示出来。

新打开 iTerm 窗口用到的两条 Applescript 分别是[^2]：

- 用默认 profile 打开：
```
create window with default profile
```

- 用指定 profile 打开：
```
create window with profile "name"
```

# #2
首先打开 Action Editor 新建一个 action，因为这个 action 有可能接受参数，
但不是必须接受参数，所以选上了 "Accepts string argument" 但没有选
"Requires argument"。

![](/content/images/2017/10/launchbar-action-options.png)

这里的参数设定在文档里有基本的说明：

https://developer.obdev.at/launchbar-developer-documentation/#/action-info-plist

语言的选择，我的第一选择是 Applescript，虽然不是专家，但毕竟简单🙄；其
次是各种脚本语言，考虑到从 Yosemite 起 Javascript(JXA) 也被作为系统脚
本语言，Javascript 是次选的优先。

回到 #1 里描述的需求，我们需要的是：

1. 一个不需要 argument 能直接运行（打开默认 profile）的操作
2. 一个能把候选 profile 名单列出来，并且随着输入实时缩小候选项的操作
3. 从 2. 的结果里选择最终需要的 profile，或者直接完成输入后，回车用当
   前 profile 打开新窗口的操作
   
## #2.1
这个最简单，按照文档的描述，所有 action 都必须有 Default Script，然后
Default Script 里的 `run` 函数会被回车键直接触发，所以只需要在 Default
Script 文件里放上这样一段就全解决了（考虑到 Default Script 里会直接操
作 iTerm，我选择了 Applescript）：

```
on run
	tell application "iTerm"
		activate
		create window with default profile
	end tell
end run
```

## #2.2
这里会遇到一个坑`(*_*)`

翻完文档之后会发现要让 LaunchBar 的 action 显示一段返回值作为候选列表
有多种选择：Default Script 里的 `handle_string` 或者 `runWithString`
函数；语焉不详的 "Live feedback enabled" 选项；还有 Suggestions Script
里的 `handle_string` 或者 `runWithString`。

文档里对这三者的区别基本没有提及，通过摸索，得出的区别是：

1. Suggestion Script 会实时接收输入的文字，换句话说，每一个字母/文字的
   输入都会触发一次脚本里的 `handle_string` 或者 `runWithString` 函数。
   函数运行的返回值会显示在交互的列表里，并等待下一次的输入。
2. Default Script 只会在回车确认输入的时候触发脚本里的 `handle_string`
   或者 `runWithString` 函数。
3. 如果 "Live feedback enabled" 被选上的话，每一次输入都会触发 Default
   Script，并显示函数的返回值，**但是回车不会触发**。

根据上面的总结的话，这里需要一个 Suggestion Script 来过滤输入，返回过
滤后的 profile 列表，然后在 Default Script 里需要有函数来接收最终的选
项（不管是直接来自输入还是来自选择列表），用对应的 profile 打开 iTerm
窗口。最后**不选** "Live feedback enabled"。

考虑到操作字符串的容易程度，我选择了用 Javascript 来写 Suggestion
Script。因为并不是很大的 profile 列表，也不是很实时的操作，作为 Y 分钟
[^3] 速成出来的 JS 小白，并没有花时间做优化`=_=`

```
function runWithString(string)
{
    var profiles = ["Light", "Dark", "Light code", "Dark code", "Light read", "Dark read"]
    var ret = [];
    var input = string.toLowerCase();
    for (var p in profiles) {
        if (profiles[p].toLowerCase().includes(input)) {
            ret.push({title: profiles[p]});
        }
    }
    
    return ret;
}
```

## #2.3
回车接收输入的 Default Script 的写法也很简单，只是跟前面的 `run` 函数略有不同：

```
on handle_string(_string)
	tell application "iTerm"
		activate
		create window with profile _string
	end tell
end handle_string
```

# #3
到这里一个 action 基本就成型了，除了文档的坑之外我觉得 LaunchBar 的
action 开发比 Alfred 的 workflow 要来得略容易一些。Alfred 的拖拽式开发
看似简单，但用起来感觉并不太直观，各种组件的用法和参数的传递都有点绕。

如果是打算操作 Terminal.app 的话过程稍微不一样一点，因为 Terminal.app 并不支持 Applescript 操作窗口和 tab，所以只能靠万能的 Applescript 菜单和键盘访问：
```
-- 默认 profile
tell application "System Events" to keystroke "n" using {command down}

-- 指定 profile profileName
click menu item profileName of menu of menu item "New Window" of menu "Shell" of menu bar 1
```

# #4
Fin.

[^1]: [https://developer.obdev.at/launchbar-developer-documentation/#/welcome](https://developer.obdev.at/launchbar-developer-documentation/#/welcome)
[^2]: [https://www.iterm2.com/documentation-scripting.html](https://www.iterm2.com/documentation-scripting.html)
[^3]: 从未写过 JS 的我表示用 [https://learnxinyminutes.com](https://learnxinyminutes.com) 来速成脚本语言挺好用的
