---
title: WP Microblogs 0.3.3 版本升级
tags:
  - WordPress
  - WP Microblogs
  - 微博
  - 插件
id: 393
categories:
  - 作品
date: 2011-08-24 23:43:29
---

这个插件是二月份写出来的, 后来由于备考一直没有时间维护升级. 期间一直有人反映无法自动更新, 但无奈我与我朋友的博客上都没有发生这种问题, 这两天一直在排查原因依然无果. 如果可以, 希望使用此插件无法自动更新的朋友能将主机帐号借给我用一段时间.

[![WP Microblogs 0.3.3 版本升级](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2011/wp-microblogs-0-3-3.jpg)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2011/wp-microblogs-0-3-3.jpg)<!-- more -->

此次升级主要针对新浪微博平台升级带来的一些问题:

1. 修正了新浪微博 ID 记录为科学计数法的问题;
2. 取消了新浪微博的 Basic 认证方式;
3. 将新浪微博域名改为 webio.com, 使用与新浪微博网页版中一致的 URL.

新浪微博 ID 变为科学计数法, 是因为微博 ID 从 35 位升级到了 52 位, PHP 自带函数 json_decode 在解码时转换成了科学计数法. 所幸不难解决, [这里](http://forum.open.weibo.com/read.php?tid=7990&amp;page=2#17402)给出了方法, 将 ID 用引号括住, 即可识别为字符串.

微博的 mid 与 URL 互转方法也被人探索出来了, 传送门在[这里](http://forum.open.weibo.com/read.php?tid=3236).

WordPress 官方插件主页: [http://wordpress.org/extend/plugins/wp-microblogs/](http://wordpress.org/extend/plugins/wp-microblogs/)
