---
layout: post
title: Emacs narrowing
date: '2016-07-03 19:25:29'
yun: 江
tags: emacs
---

Emacs narrowing 可以只显示 buffer 的一部分而隐藏其他，不过无意中触发出来的话往往会把不熟悉这个功能的用户吓一跳（比如我`(｡-_-｡)`

它的用法是

- narrow（隐藏）
  * `C-x n n` 隐藏 region
  * `C-x n p` 隐藏当前页 (page)
  * `C-x n d` 隐藏当前的 defun，应该是写 elisp 的时候用到的比较多

- wide（显示）
  * `C-x n w` 显示整个 buffer
