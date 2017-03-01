---
title: Final Password
tags:
  - 安全
  - 密码
id: 415
categories:
  - 分享
date: 2012-04-18 13:51:01
---

考研大幕已落, 初试第三, 复试后排到第二, 特此纪念.

前段时间的 CSDN 密码泄漏事件引起广泛关注, 我的邮箱与密码也赫赫在上. 由于我只使用两个密码, 面对莫须有的危机, 还是决定试用密码管理软件. 现在简要介绍两款.

[![Final Password](//img.beamnote.com/2012/final-password.jpg)](//img.beamnote.com/2012/final-password.jpg)<!-- more -->

## 一、LastPass

LastPass 是一款来自美国的密码管理软件. 相比类似的软件, 它有如下的优势 / 特点:

* 密码储存于本地与线上, 不必担心丢失的问题;
* 多系统与浏览器支持, 包括 Windows、Mac OS、Linux、Android、iOS、webOS、BlackBerry、Symbian、Windows Phone 与 Internet Explorer、Firefox、Safari、Chrome、Opera;
* 所有密码在本地加密;
* 提供用户名、密码的自动输入功能;
* 提供「建立安全密码」功能;
* 电脑版与移动网页版完全免费, 移动应用需要高级帐户, 按月收费.

LastPass 号称「是你需要记住的最后一个密码」, 这意味着「是别有用心的人需要窃取的唯一一个密码」. 我们使用 LastPass 是为了提高安全性, 而不是大开方便之门, 因此我的使用策略是:

* 建立「一般的」与「重要的」分组, 对于「重要的」分组, 定期更换密码;
* 使用 LastPass 内建的「建立安全密码」功能生成并储存强壮的密码;
* 对于涉及资金交易的网站, 例如「支付宝」, 不使用 LastPass 管理密码;
* 对于「重要的」分组中的网站, 开启「需要密码重新提示」.

顺便提一下, 对于拥有教育邮箱的用户, 可以通过下面的链接免费获得半年高级帐户, 以前开放申请的武汉理工 Live 邮箱也可以使用.

Last <3's Students: [https://lastpass.com/edupromo.php](https://lastpass.com/edupromo.php)

## 二、花密

第一次听说花密, 是在一个介绍新网站的微博上. 当时我不清楚花密的原理, 号召大家使用 LastPass, 结果把作者引来了. 花密与 LastPass 不同, 本身不储存密码, 而是在使用时生成密码.

首先, 依然需要一个「记忆密码」, 然后根据不同的网站填写「区分代号」, 使用花密计算最终密码.

[![花密](//img.beamnote.com/2012/flowerpassword.png)](//img.beamnote.com/2012/flowerpassword.png)

花密目前有网页版、Windows 版、Chrome 扩展、手机网页版、Android 版, 按[这里](http://flowerpassword.com/app/web)可以使用或者下载花密.

花密的加密也比较有意思, 其根据两个 Key 生成两个 MD5, 一个作为源, 在另一个表里查找是否需要转换为大写. 如果源的第一个是数字, 则换成 K 开头. 其结果就是, 如果不是 K 开头, 取 MD5 的前 16 位; 如果是 K 开头, 则为 「K」 与 MD5 的 2~16 位.
