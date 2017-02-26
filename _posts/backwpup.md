---
title: "不错的备份插件: BackWPup"
tags:
  - Dropbox
  - WordPress
  - 博客
  - 备份
  - 插件
id: 395
categories:
  - 分享
date: 2011-08-29 14:05:31
---

前天将博客从洛奇亚的 VPS 中搬了出来, 新家安在老薛主机. 老薛主机给我的印象是实惠, 而且服务器配置不错. 但是老薛主机似乎没有自动备份, 我记得胡戈戈主机会每天备份一次, 用户可以自主拿到相当长一段时间之内的备份.

[![BackWPup](http://img.beamnote.com/2011/backwpup.jpg)](http://img.beamnote.com/2011/backwpup.jpg)<!-- more -->

现在 Cloud 的概念仍在被炒作, 其实这东西在我看来不过是网盘 + 服务罢了. Dropbox 是最有名的云在线存储服务, 尽管在中国不稳定, 但在美国的服务器访问 Dropbox 可是快马加鞭. 如果使用 Dropbox 进行备份, 配合 Dropbox 客户端, 即实现了本地与云端双保险.

WordPress 有很多插件可以将网站备份到 Dropbox. 目前可以搜索到的插件中, 「WordPress Backup to Dropbox」和「wp Time Machine」会把整个网站和数据库都丢到 Dropbox 里去, 似乎都不能定制. 另一个问题是它们似乎会在 wp-content 目录中生成临时文件, 如果不采取额外措施, 有被非法下载的风险.

BackWPup 插件看起来最符合我的要求, 我总结的优点与特点如下:

* 可以自由选择需要备份的内容;
* 本地的备份文件可以放置在不能被 HTTP 访问的目录中;
* 发生错误时发送邮件通知;
* 数据库被压缩之后才会丢到 Dropbox.

官方列出的特点如下:

* 数据库备份;
* 可以导出 WordPress XML 文件, 即 WordPress 控制板中的「导出」功能;
* 优化数据库;
* 检查 / 修复数据库;
* 文件备份;
* zip, tar, tar.gz, tar.bz2 格式任选 (phpMyAdmin 只接受 .gz 格式, 这里压缩的 .tar.gz 也不能被 phpMyAdmin 正确使用);
* 存放备份到文件夹 / FTP 服务器 / Amazon S3 / Google Storage / Microsoft Azure (Blob) / RackSpace Cloud / Dropbox / SugarSync / 邮箱;
* 发送日志到邮箱.

功能太强大, 所以缺点就是臃肿 :-( , 还有, 至少需要 PHP 5.2.5 和 WordPress 3.1.

WordPress 官方插件主页: [http://wordpress.org/extend/plugins/backwpup/](http://wordpress.org/extend/plugins/backwpup/)

我会告诉你下面是翻墙注册了以后你和我都能得到 250MB 空间的 Dropbox 推广链接吗?

[https://www.dropbox.com/referrals/NTU3OTM4MTE5](https://www.dropbox.com/referrals/NTU3OTM4MTE5)
