---
title: 让网站统计屏蔽自己的流量
id: 147
categories:
  - 技巧
date: 2010-04-11 08:00:39
tags:
---

很多博客主都有统计网站流量的习惯, 谷歌分析、CNZZ 等都是不错的选择. 但是统计代码无法区分博客主与访客的访问, 站长访问产生的流量也被统计在内, 使统计结果变得不准确.

[![被无情抛弃的站长](//img.beamnote.com/2010/forbidden-my-own-flow.jpg)](//img.beamnote.com/2010/forbidden-my-own-flow.jpg)<!-- more -->

对于 WordPress, 我们只要在统计代码前后插入很简短的一段代码就可以屏蔽自己访问产生的流量:

{% codeblock lang:php %}
<?php if (!$user_ID) { ?>
<!-- 此处插入统计代码 -->
<?php } ?>
{% endcodeblock %}

其中, user_ID 返回了当前登录用户的 ID, 如果是未登录用户则返回 0, 所以, 只要您当前已登录, 统计代码就不会工作.

如果是开放注册的网站, 只需要屏蔽管理员的 user_ID 即可, 例如:

{% codeblock lang:php %}
<?php if ( !( $user_ID == 1 || $user_ID == 2 ) ) { ?>
<!-- 此处插入统计代码 -->
<?php } ?>
{% endcodeblock %}
