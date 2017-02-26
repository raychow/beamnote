---
title: 自动使用 Google SSL 加密搜索
tags:
  - Chrome
  - Google
  - 下载
id: 180
categories:
  - 作品
date: 2010-05-25 20:32:06
---

[![SSL Certificates Pro for Chinese](http://img.beamnote.com/2010/goo-gle-ssl-greasemonkey.jpg)](http://img.beamnote.com/2010/goo-gle-ssl-greasemonkey.jpg)<!-- more -->

这个脚本已经停止更新, 您可以通过修改 hosts 文件将 Google 解析到 Google.cn 的 IP, 请参考 SmartHosts 项目.

相关链接:

项目主页: [https://code.google.com/p/smarthosts/](http://code.google.com/p/smarthosts/)

hosts 文件: [https://smarthosts.googlecode.com/svn/trunk/hosts](https://smarthosts.googlecode.com/svn/trunk/hosts)

如果不能访问 Google Code, 可以访问镜像站: [https://smarthosts.sinaapp.com/](https://smarthosts.sinaapp.com/)

<!--nextpage-->

**Google 加密搜索已被 DNS 劫持, 请在 hosts 里添加 209.85.225.104 encrypted.google.com 继续使用Google加密搜索, 也可将 IP 换成自己 ping **[**www.google.com**](http://www.google.com/)** 得来的 IP. **

## 描述 Description

从 [SSL Certificates Pro](http://userscripts.org/scripts/show/72944) 修改而来, 自动将 Google 搜索、Google Reader、Google Docs 等 Google 提供的支持加密连接的服务切换至加密连接, 并将搜索结果中的网页快照也替换为加密连接.

目前已经支持 Firefox、Chrome 与 Opera 浏览器, **IE 用户请安装 1.3.0 版. **

Firefox 与 Opera 无须担心连接重置的状况, 但 Chrome 与 IE 在非加密连接已重置的状况下无法跳转.

请注意:

**Opera 用户** 需要在首选项编辑器中打开 User JavaScript on HTTPS (访问 opera:config#UserPrefs|UserJavaScriptonHTTPS) 设置, 否则会发生无限跳转现象.

**IE 用户** 需要将 [https://www.google.com](https://www.google.com/) 添加到可信站点, 否则会发生无限跳转现象.

**要指定默认搜索语言, 请修改脚本中 defaultLanguage 变量. **

Automatically switch Google search, Google Reader, Google Docs and other Google services to encrypted connection, and the snapshot of the search results page is also replaced with an encrypted connection.

It supports Firefox, Chrome and Opera browser. **For Internet Explorer users, please install the 1.3.0 version.**

Please note:

**Opera user** needs to enable the User Javascript on HTTPS (Visit opera:config#UserPrefs|UserJavaScriptonHTTPS) setting in opera:config.

**IE user** needs to add [https://www.google.com](https://www.google.com/) to the Trusted Zone.

**To specify the default search language, modify the defaultLanguage variable in the script.**

## 更新 Update Log

2.1.0

去掉了即使加密连接也不能上的网站.

2.0.0

将加密搜索连接 [https://www.google.com](https://www.google.com/) 换为 [https://encrypted.google.com/](https://encrypted.google.com/).

Replace [https://encrypted.google.com](https://encrypted.google.com/) by [https://www.google.com](https://www.google.com/).

1.3.5

在加密 Google 搜索页面提供导航链接条 (只有英文版). 感谢 realfiona

Provides the navigation bar in encrypted Google search pages. (English only)

1.3.4

根据 google.com.hk 搜索地址自动检测当前使用的语言;

自动设定 Cookies 中 google.com 的语言, 以后访问 google.com 将不会变英文.

Automatically detect the current language according to search address of google.com.hk.

1.3.2

在非加密 Google 首页提供加密页面链接;

在加密 Google 首页提供非加密搜索左上角的导航链接条.

Provides a link to encryption version in the non-encrypted Google homepage;

Provides the navigation bar in the encrypted Google homepage.

1.3.0

此版本支持 Internet Explorer.

This version can running in Internet Explorer.

1.2.4

隐藏了 [https://www.google.com/ncr](https://www.google.com/ncr) 跳转页面;

可以设定 Google 搜索使用的语言.

Hide [https://www.google.com/ncr](https://www.google.com/ncr) jump page;

You can set the language of Google Search now.

1.2.2

网页快照中切换「纯文字版本」与「完整版本」的链接自动使用加密连接. 感谢 Randy

"Text-only version" and "Full version" links automatically use encrypted connection in the snapshot.

1.2.0

解决查看快照时出现「重定向声明」的问题. (测试版本) 感谢 baidusaid, Randy

Remove "Redirect Notice" page when you click the "Cached". (Beta)

1.1.0

现在于 google.com.hk 搜索时自动跳转到 google.com.

Automatically jump to google.com when you visit google.com.hk.

1.0.0

基本功能.

Basic functions.
> 按这里下载: [http://userscripts.org/scripts/show/77725](http://userscripts.org/scripts/show/77725)
使用示例:

[![Google SSL 搜索的油猴脚本](http://public.blu.livefilestore.com/y1pVlAI27BMYnoUJDudSdZJxCJdJQgPS0WIkX_1RB3mXjLUk3c2mOFyqYPUOi9NwfbE-DeN0C-xhZhjav6pOzt-Jw/Google_SSL_Serach.png)](http://public.blu.livefilestore.com/y1pVlAI27BMYnoUJDudSdZJxCJdJQgPS0WIkX_1RB3mXjLUk3c2mOFyqYPUOi9NwfbE-DeN0C-xhZhjav6pOzt-Jw/Google_SSL_Serach.png)
