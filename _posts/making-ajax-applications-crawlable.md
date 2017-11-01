---
title: 让 AJAX 程序可被抓取
tags:
  - AJAX
  - Google
  - 网络
id: 275
categories:
  - 技巧
date: 2010-12-18 20:50:36
---

这是 AJAX 程序系列第二波了, 上篇的结尾提到, 我将针对 Google Code 中[《Making AJAX Applications Crawlable》](http://code.google.com/intl/en-US/web/ajaxcrawling/docs/learn-more.html)与大家一起探讨, 不过是将这篇文章翻译成中文 8-).

Google 说, 如果您希望 AJAX 程序动态生成的内容出现在搜索结果中, 我们有一个方法帮助 Google (和潜在的其它搜索引擎, 当然百毒是不靠谱的) 检索和索引您的内容.

[![让 AJAX 程序可被抓取](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/making-ajax-applications-crawlable.jpg)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/making-ajax-applications-crawlable.jpg)<!-- more -->

## 一、目前的做法

目前, 很多网站管理员创建一个「平行宇宙」的内容, 打开 JavaScript 的浏览器用户将看到动态生成的内容, 而无 JavaScript 的浏览器用户与蜘蛛们将看到准备好的被脱机创建的静态内容.

如果您的网站刚刚开始, 一个好的方法是构建网站结构和导航时只使用 HTML, 当网站的页面、链接和内容都构建好之后, 你可以使用 AJAX 为表现加分. Googlebot 会很乐意看见 HTML, 而现代浏览器将能享用 AJAX.

当然您可能有链接需要 JavaScript  以实现 AJAX 功能, 这里有一个方法帮助 AJAX 与静态链接共存, 如:

{% codeblock lang:html %}
<a onclick="navigate('ajax.html#foo=32'); return false" href="ajax.htm?foo=32">foo 32</a>
{% endcodeblock %}

请注意, 静态链接的网址有一个参数 (?foo=32) 而不是一个片段 (#foo=32), 片段是使用在 AJAX 代码中的. 这一点很重要, 因为搜索引擎能理解参数而往往忽略片段. 网路开发人员 Jeremy Keith 把这种方法称为 `Hijax`. 在您提供静态链接后, 用户和搜索引擎都可以链接到希望共享或引用的具体内容.

这种做法一直有效, 如果您的网站已经配置了 `Hijax`, 可以不再看本文了. 但是, 如果您的内容会定期更换却不想手动更新这两个链接, 或者您希望搜索引擎能快速服务 AJAX 链接, 或者您还没有实现 `Hijax` 的计划, 您可以考虑下面描述的新计划.

## 二、蜘蛛与服务器间的协议

为了让 AJAX 程序被抓取, 您的网站必须遵守新的协议. 该协议依托于以下几点:

1. 该网站已经采用本计划 (废话 :-? );
2. 对于有动态生成内容的每个网址, 服务器需要提供一份用户可以看到的内容的 HTML 快照. 通常, 这样的网址是 AJAX URL, 也就是 URL 中包含了 hash 片段, 例如 `www.example.com/index.html#key=value`, 其中 `#key=value` 就是 hash 片段. 一份 HTML 快照指在 JavaScript 执行后页面出现的内容;
3. 搜索引擎索引 HTML 快照并在搜索结果中提供 AJAX 原始 URL.
为了完成这项工作, 程序必须在 AJAX URL 中使用特定语法 (姑且称之为「漂亮的网址」), 搜索引擎会暂时把这些「漂亮的网址」修改为「丑陋的网址」, 并使用被修改过的网址向服务器发出请求. 服务器不应该返回提供给浏览器的正常的网页, 而是一个 HTML  快照. 当蜘蛛取得了从「丑陋的网址」返回的内容后, 在搜索结果显示「漂亮的网址」. 换句话说, 最终用户将总是看到一个包含 hash 片段的「漂亮的网址」, 如下图所示:

[![An agreement between crawler and server](http://code.google.com/intl/en-US/web/ajaxcrawling/docs/images/overview.png)](http://code.google.com/intl/en-US/web/ajaxcrawling/docs/images/overview.png)

## 三、实施

### 1\. 向蜘蛛说明您的网站支持 AJAX 抓取计划

要做到这一点, 只要使 hash 片段以 \!(惊叹号) 开头. 例如, 您的 AJAX 程序包含一个这样的 URL:

`www.example.com/ajax.html#key=value`

那么它应该被修改成这样:

`www.example.com/ajax.html#!key=value`

这将意味着如果您的网站提供 HTML 快照, 蜘蛛将能看到应用程序生成的内容.

## 2\. 配置服务器

您的服务器需要能处理 URL 中包含 `_escaped_fragment_` 的请求.

假设您希望得到 `www.example.com/index.html#!key=value` 的索引, 该协议的一部分是提供此 URL 的 HTML  快照, 以便蜘蛛能够看到内容. 如何让您的服务器知道何时返回一份 HTML 快照而不是正常的网页? 答案是, 蜘蛛将修改每个 AJAX 的地址, 如:

`www.example.com/ajax.html#!key=value`
暂时改为

`www.example.com/ajax.html?_escaped_fragment_=key=value`

这样做是因为:

1. 按规范, hash 片段从不作为 HTTP 请求的一部分发送到服务器. 换句话说, 蜘蛛需要一个让服务器知道它想要的内容的 URL `www.example.com/ajax.html#!key=value` 的方法 (而不是简单的 `www.example.com/ajax.html`);
2. 另一方面, 服务器需要知道它必须返回一份 HTML 快照而不是给浏览器的正常页面.

**注意: 蜘蛛会在传输时对片段中的字符进行编码, 要正确的检索, 请务必解码所有片段中所有的 %XX 字符. 具体来说, %26 应该解码为 &amp;, 20% 应该解码为空格, 25% 应该解码为 %, 等等. **

下面就是制作 HTML 片段了, 有很多种方法, 这里只是其中一些:

* 如果有很多 JavaScript 生成的内容, 您可能需要一个「无头浏览器」 (headless browser) 来生成 HTML 快照, 如 [HtmlUnit](http://htmlunit.sourceforge.net/); 或者, 您可以用使用不同的工具, 如 [crawljax](http://www.crawljax.com/) 或 [watij.com](http://www.watij.com/);
* 如果大多数内容由服务器端技术生成, 如 PHP 或 ASP.NET, 您可以使用现有的代码, 只将 JavaScript 部分替换为静态或服务器端生成的 HTML;
* 您可以离线创建一个页面的静态版本. 例如, 许多应用程序从数据库中拉取内容并在浏览器中呈现. 相反, 可以为每个 AJAX 地址创建一个单独的 HTML 页面.

使用 [Fetch as Googlebot](http://www.google.com/support/webmasters/bin/answer.py?hl=en&amp;answer=158587) 查看 HTML 快照是否正确产生.

## 3\. 抓取没有 hash 片段的页面

有些页面可能没有 hash 片段, 例如, 您希望主页是 `www.example.com` 而不是 `www.example.com#!home`. 出于这个原因, 对没有 hash 片段的页面有一个规定: 在 HTML 头部添加一个特定的元标记, 形式如下:

{% codeblock lang:html %}
<meta name = "fragment" content = "!" >
{% endcodeblock %}

换句话说, 如果在 `www.example.com` 首页中插入 `<meta name = "fragment" content = "!" >`, 蜘蛛将会临时将网址映射到 `www.example.com?_escaped_fragment_=` 并从服务器请求这个网址.

! 是 content 唯一有效的值.

## 4\. 考虑更新 Sitemap

蜘蛛使用 Sitemap 帮助补完爬行. 您的 Sitemap 应该包含希望在搜索引擎中显示的网址, 所以也应该加上 `http://example.com/ajax.html#!key=value`, 但不要加上 `http://example.com/ajax.html?_escaped_fragment_=key=value` 的形式, Google 会将它作为一个单独的页面列出来.

## 5\. 通过 [Fetch as Googlebot](http://www.google.com/support/webmasters/bin/answer.py?hl=en&amp;answer=158587) 检查

这是在谷歌网站管理员工具中提供的.

[深入了解生成 HTML 快照.](http://code.google.com/intl/en-US/web/ajaxcrawling/docs/html-snapshot.html)
