---
title: "老调重谈: SyntaxHighlighter 代码高亮"
tags:
  - WordPress
  - 代码
  - 博客
id: 330
categories:
  - 技巧
date: 2011-01-27 21:03:15
---

> 题外话: 上次提到的显示各种微博的 WordPress 插件进度可喜, 已经完成了九个微博的 OAuth、Basic HTTP 验证部分.
写这篇文章时我非常郁闷, 因为这篇本是介绍如何智能判断加载 SyntaxHighlighter 代码高亮脚本的. 我有一个习惯, 不喜欢写太多别人写烂的东西. 在搜索 WordPress 插件库时, 我发现了 [Auto SyntaxHighlighter](http://www.akii.org/auto-syntaxhighlighter.html) 这款插件, 完完全全的实现了期望的内容——自动根据本文出现的代码加载相应脚本, 甚至更强. 既然这样, 我就告诉大家选择 SyntaxHighlighter 的理由吧.

[![SyntaxHighlighter](//img.beamnote.com/2011/syntaxhighlighter.jpg)](//img.beamnote.com/2011/syntaxhighlighter.jpg)<!-- more -->

之前我使用代码发芽网高亮代码, 先将代码贴到代码发芽网, 网站将代码高亮并给出 HTML  代码, 再贴到编辑器中. 我不太喜欢的是:

1. 在编辑器中会存在大量有 `style` 属性的 `div` 标签, 编辑文章时很麻烦;
2. 如果显示行号, 复制时会将行号也复制下来;
3. 没有滚动条, 不能左右滚动;
4. 如果使用 WordPress 自带编辑器, 容易导致行首空格消失——除非你不进入「可视化」编辑模式.

我认为, 代码发芽网更适合限制较多的托管博客使用.

WordPress 有一些代码高亮插件, 如 SyntaxHighlighter Evolved, 它们的启用方式是将代码使用 `[language]` 与`[/language]` 包围, 这也有一点问题:

1. 如上面第四点所述, 行首空格经常消失;
2. 使用 WordPress ShortCode (即以中括号包围的标签) 存在移植问题: 将文章从 WordPress 博客转移到其它博客, 将留下 `[language]` 标签并会直接显示; 即使仍然使用 WordPress, 停止使用这款插件也会导致标签被显示.

比起 `div` 与 WordPress ShortCode, HTML 标签 `pre` 中的文本通常会保留空格和换行符. 因此, 支持 `pre` 形式的代码高亮方式是最佳选择.

有两款插件通过了筛选, 一是 Auto SyntaxHighlighter, 另一个是 WP-Syntax.

* Auto SyntaxHighlighter 通过 JavaScript 高亮代码, 占用服务器资源较少 (仅用于判断是否加载 JavaScript), 但不能高亮 RSS 输出;
* WP-Syntax 在服务器端高亮代码, 占用较多资源, 但可以在 RSS 中实现高亮, 且不受浏览器禁用 JavaScript 的影响 (当然这种情况很少发生).

我更倾向减少服务器资源占用, 因此选择 Auto SyntaxHighlighter. 它还有一大杀手锏, 可以为 TinyMCE 编辑器添加一个插入代码的按钮, 不仅不用记忆开启高亮的标签格式, 也可以自动实体化 < > 等符号, 贴心又方便.

[![Auto SyntaxHighlighter](//img.beamnote.com/2011/auto-syntaxhighlighter.jpg)](//img.beamnote.com/2011/auto-syntaxhighlighter.jpg)
