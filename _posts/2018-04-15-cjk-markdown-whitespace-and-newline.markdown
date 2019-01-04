---
title: 东亚文字 Markdown 换行变空格的问题
layout: post
date: 2018-04-15 16:40:45
yun: 删
comments: true
tags: blog
---

最近新遇到一个老问题。

在把大部分笔记转换成文本格式的时候我开始让自己习惯用 Emacs 的 `fill-paragraph`
来保持对齐。跟现代编辑器常见的“折行”不同，`fill-paragraph` 的功能是用“硬”换行来
保持每行对齐在一定宽度。这样写完之后即使用其他的工具打开也能保持原有的格式，例子
的话可以参考文本的 [RFC](https://www.ietf.org/rfc/rfc1.txt)。另外用这种方法也是
为了规避 Emacs 以及其他不少文本编辑器在折行上的一个问题：不能很好的判断中日韩文
字的分词，只能按照空格来折行。

其他的都还好，但在写 blog 的时候遇到一个问题，markdown，或者更准确的说是
HTML/CSS 在渲染段落的时候，会自动把换行替换成空格[^1]。这对于英文或者其他空格间
隔单词的字母文字来说是正确的，但对于不用空格间隔单词的中日韩文来说就会带来一个多
余的空格：

![换行变空白的代码](/content/images/2018/04/newline-to-space-src.png)  
![换行变空白的例子](/content/images/2018/04/newline-to-space.png)

上面是 HTML 的代码，注意段落中的换行是在“是个”跟“有点”之间；下面是渲染的结果，可
以看到这个换行变成了句子里多余的空格。

这个问题虽然不热门，但仔细搜一搜也能找到不少讨论，解决方案不外乎两个思路

1. 在 markdown 编译之前处理一遍源文件，去掉硬换行。
2. 修改 jekyll 或者其他静态网站生成器，或者利用插件，在最终生成 HTML 的时候去掉
   段落里的换行。我之前用过这个选项[^2]

但 markdown 只是按照简单的规则对 HTML 的简写，这个问题归根结底是 HTML/CSS 没有处
理好中日韩等（好奇还有没有“等”？）东亚文字的段落。所幸 CSS3 的草案中已经有对这个
问题的考虑[^3]，目前来看，Firefox 59 已经有能看得到的支持，Safari 11.1 和 Chrome
65 还没有。

所以结论是我不打算在这个问题上花精力了，典型的不是我是世界的错`(´▽｀)`，等 CSS3
和各大渲染引擎的实现吧。实在有碍观瞻的文章我会用一个 `unfill-paragraph` 的函数跑
一遍，其他的就请优先使用 Firefox 或者忍忍好了🙃。

Fin.


[^1]: 假设是 `white-space：normal;` 的情况，常见、也让描述简单一些。
[^2]: <http://blog.guorongfei.com/2015/04/25/how-to-fix-the-markdown-newline-blank-problem/>
[^3]: <https://drafts.csswg.org/css-text-3/#line-break-transform>
