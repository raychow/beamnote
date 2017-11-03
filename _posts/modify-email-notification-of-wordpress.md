---
title: 对 WordPress 留言邮件通知的小修改
tags:
  - WordPress
  - 博客
id: 280
categories:
  - 技巧
date: 2011-01-05 21:58:31
---

WordPress 会把访客的留言通过邮件通知管理员, 结合邮件检查程序我们就能随时保持互动, 但这个邮件通知的程序存在一些不足, 稍微修改就能臻于完美.

[![对 WordPress 留言邮件通知的小修改](//img.beamnote.com/2011/modify-email-notification-of-wordpress.jpg)](//img.beamnote.com/2011/modify-email-notification-of-wordpress.jpg)<!-- more -->

我认为需要改进的地方有:

1. 提供的 WHOIS 查询链接是个鸡肋, 不仅查不到 IP 详细信息, 速度还慢;
2. 查看评论的链接没有分页, 只能跳转到最新评论页, 对于查看较早时候评论的回复不方便.

改动的很简单啦\! 只是要修改 WordPress 的核心文件, 修改之前备份好, 改坏了后台都进不去 :-? .

邮件通知的代码在 `wp-includes/pluggable.php` 中.

## 一、改进 IP 查询

其实就是把 WHOIS 链接换掉, 我使用 IP138 查询 IP, 网址可以自行修改.

1. 打开 `wp-includes/pluggable.php`, 搜寻 `whois`, 会有两个结果, 都需要修改, 第一个结果位于为正常的评论、Trackback、Pingback 发送邮件的函数, 第二个结果位于为审核评论发送的邮件的函数;
2. 在两处搜寻结果的下方分别插入一行:

{% codeblock lang:php %}
comment_author_IP ). "\r\n";
{% endcodeblock %}

## 二、改进评论链接

修改后, 邮件中提供的查看评论的链接将直接指向对应评论页面.

搜寻

{% codeblock lang:php %}
comment_post_ID). "#comments\r\n\r\n";
{% endcodeblock %}

将此行替换为

{% codeblock lang:php %}
comment_ID). "\r\n\r\n";
{% endcodeblock %}

完工
