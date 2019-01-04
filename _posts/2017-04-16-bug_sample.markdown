---
title: Bug Sample
layout: post
date: 2017-04-16 19:57:11
yun: 铣
comments: true
tags: c++
---


周五下午 code review 的时候看到个挺有意思的问题。

我们代码里有个地方，在释放一段内存之前需要做一个有点复杂的判断，所以有人把这部分的判断逻辑拿出来放到一个函数里，这个函数长这样：

```
bool foo()
{
  bool rc = false;
  if (<condition1>)
    rc = true;
  if (<condition2>)
    rc = true;
  <...>
}
```

具体的判断逻辑比这个复杂得多，不过在这里不重要，大概看个结构就行了。

代码的问题在于它把最后一个 `return rc;` 给丢了`ˊ_>ˋ`，所以函数的返回值是未定义的行为，具体的值完全取决于编译器的实现。这个看 `-S` 出来的汇编也能看得出来，至少 g++ 和 clang 上已经是两个不同的结果了。

写这个代码的是组里资格相当老的一个开发，这段代码已经被 code review 过一遍没有发现问题，valgrind 报警之后再次换人 review，这才被找了出来…