---
title: 手下留情
tags:
  - 攻击
  - 网站
  - 网络
id: 319
categories:
  - 唠叨
date: 2011-01-23 05:34:09
---

最近怪事特别多, 关于我的网站, 总是遭遇一些摸不清头脑的事情. 唉, 我是招你惹你了?

[![龙猫和麦当劳](//img.beamnote.com/2011/totoro-and-mcdonald.jpg)](//img.beamnote.com/2011/totoro-and-mcdonald.jpg)<!-- more -->

## 一、奇怪的 Feed 访问

换了主机之后的那两天, 心血来潮的想要统计一下网站订阅量, 装了 [Feed Statistics](http://www.chrisfinke.com/wordpress/plugins/feed-statistics/).

第二天, 我惊奇的发现有很多 IE6 访问过我的订阅, 同时, 访问的订阅地址也是奇怪的形如 `http://beamnote.com/feed?1057199990` 的参数形式.

进入数据库筛选了一下详细的订阅情况, 原来是 `60.28.113.193` 与 `60.28.113.196` 这两个 IP 大约二十分钟轮流访问我的订阅地址. 观察了几天也没有看出什么门道, 有可能是采集站, 偏偏这两个 IP 又不能访问, 果断封锁.

[![奇怪的 Feed](//img.beamnote.com/2011/strange-feed.png)](//img.beamnote.com/2011/strange-feed.png)

封锁 IP 对于 Apache User 来说应该是轻车熟路了, 在 `.htaccess` 里加上这两句轻松搞定.

{% codeblock lang:bash %}
Deny from 60.28.113.193
Deny from 60.28.113.196
{% endcodeblock %}

## 二、恶意攻击

说起这个更来气, 不知道何人申请了一免费空间, 写了一段恶意代码无休止循环下载我的主题样式文件, 以一秒访问几十次的速度, 两天不到消耗完 30G 流量, 等我赶回来主机帐号已经被停权了 :-x .

赶紧联系主机客服, 态度还是挺好的 :-| , 让我把主题的文件夹名字给改了试试, 还给我免费加了 5G 流量, 如果那人不再出怪招是够用到月底的.

此主机的 IP 是 `67.221.186.203`, 查了一下, 属于 LimeDomains 的免费主机. 我提交了一个 ticket 说明它的主机由恶意代码攻击我的主机, 不知道是我英语太烂还是怎么了, 他回信告诉我我的网站没有恶意代码 :-x , 还热心的给我一个 Google SB 的链接加以佐证. 我不得不又回复了一句: I mean there is malicious code on 67.221.186.203 ( It's your host ), this malicious code is always attack my host. 希望他能看懂咯……

我怎么对付这个 IP 呢? 先是把它封锁了, 但就我所学知识来看, 即使只返回一个 403 状态码, 再加上 HTTP 头、TCP 头、IP 头, 每次访问也会造成一定的流量损失, 就算只有几百字节, 如此频繁的访问也会消耗不少.

例如, 我的主机返回 403 状态码数据报的大小为 583 字节, 即使一秒只访问 10 次, 则

一天消耗的流量 = 583 × 10 × 60 × 60 × 24 B = 503,712,000 B = 491,906.25 KB = 480 MB

还剩下的几天 5G 肯定不够它消耗

看来我得出大绝招了\! 这种攻击的目的就是要消耗主机的流量, 应该是要把文件完整下载的. 如果我把来自这个 IP 的请求重定向到其它地方不就 OK 了么? 动用百毒音乐搜索, 点一首 Lady Gaga 的 Bad Romance 送给这位仁兄:

{% codeblock lang:bash %}
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{REMOTE_ADDR} 67.221.186.203
    RewriteRule (.*) http://LADYGAGASMUSICHERE [NC,L,R]
</IfModule>
{% endcodeblock %}

再查看访问记录, 频率已经下降到两三秒一次了, 效果比较显著.

后来把链接指向瑞星娱乐的下载地址 (90M 大体积), 效果更好.

如果是攻击我网站的仁兄, 拜托你高抬贵手吧, 我乃屁民学生一个, 实在不值得您攻击啊.
