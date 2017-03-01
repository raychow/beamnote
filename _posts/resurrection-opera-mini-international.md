---
title: 复活 Opera Mini 国际版
tags:
  - Opera
id: 12
categories:
  - 技巧
date: 2010-02-03 02:35:27
---

Opera 是我比较喜欢的浏览器之一, 因为相比 IE 它速度快很多, 而且 Opera Mini (Opera 的中低端手机版本) 国际版还提供了增进中国大陆人民了解境外文化 (包括香港、澳门、台湾) 的机会, 所以无论使用多么高级的手机我始终会安装这个短小精悍的 Java 版本浏览器.

可惜的是, 近日登入 Opera Mini 国际版连线时软件提示要求安装 Opera Mini 中国版, 国际版停止了对中国大陆的服务. Opera 公司为了一己之利, 置广大中国网民切身利益于不顾, 严重伤害了中国人民的感情. 但 Opera 公司这种卑劣的伎俩是阻挡不了中挪人民之间的友谊的, 人们纷纷点开谷歌的页面, 搜寻使用 Opera Mini 国际版的方法, 打破 Opera 公司对我国的封锁.<!-- more -->

要想复活 Opera Mini 国际版, 首先需要一个有国外 IP 的中转服务器, 这个服务器伪装成 Opera Mini 与其服务器交换数据, 并把数据传回手机; 其次, 需要一个修改版的 Opera Mini, 能够在每次运行前选择用于传送的中转服务器.

[Opera Mini Mod 中文网](http://opm.kuruan.net/) 是一个专门为 Opera Mini 爱好者服务的网站. 其中提供了修改版本下载, 在文章发布时最新版本为 Opera Mini 5 beta 2 自定义服务器, 中文版软件下载页面请 [按此](http://opm.kuruan.net/show.asp?id=98) 进入.

在上面提供的这个软件中修改者已经内置了一些服务器, 你可以直接使用, 或者按下面的步骤搭建自己的服务器并开始使用:

> 1. 申请国外免费虚拟主机;
> 2. 上传用于交换数据的 index.php 文件, 按此[下载](http://opm-server-mirror.googlecode.com/files/opm_php.zip); _<span style="color: #888888;">文件来源: [Opera Mini Server Mirror](http://code.google.com/p/opm-server-mirror/)</span>_
> 3. 在手机端安装修改版本的 Opera Mini, 并设定自己的服务器地址.

国外有很多免费的虚拟主机可供申请, [Byteact](http://www.byteact.com/) 是一家不错的虚拟主机供应商, 其免费主机包括了完整的域名、PHP、MySQL 支持, 拥有 1.5GB 存储空间和每月最多 50GB 的流量, 足够当做 Opera Mini 中转服务器使用了. 它不是空间最大的一家, 我推荐它是因为它名字很好记, 不是由奇怪的英文 + 数字组合构成.

点击 [这里](http://www.byteact.com/signup.php) 直接进入 Byteact 免费主机的注册页面, 填写要申请的域名、你的邮箱、网站分类 (这个不用管) 、语言, 单击 「I agree」.

注意: 国内很多邮箱无法接收由 Byteact 发来的邮件, 如 QQ 邮箱, 因此建议使用 GMail、Hotmail 等国外邮箱.

[![byteact_reg1](//img.beamnote.com/2010/byteact-reg-1.jpg)](//img.beamnote.com/2010/byteact-reg-1.jpg)

再键入两次验证码之后, 提示邮件发送成功.

收到邮件后点击其中的链接, 再次键入验证码.

下一步的页面告知了登陆使用的用户名、密码, 请牢记.

[![byteact_reg2](//img.beamnote.com/2010/byteact-reg-2.jpg)](//img.beamnote.com/2010/byteact-reg-2.jpg)

点击 「Click here to log into you VistaPanel」登陆控制台, 键入刚才提供的用户名、密码, 选择简体中文 「Chinese_simplified」并登陆.

建议立即修改密码防止密码遗忘, 密码修改完成以后要求重新登陆.

[![byteact_reg3](//img.beamnote.com/2010/byteact-reg-3.jpg)](//img.beamnote.com/2010/byteact-reg-3.jpg)

点击两处的「文件管理器」均可以进入上传文件的后台.

进入 htdocs 目录, 点击 「Upload」.

[![byteact_reg4](//img.beamnote.com/2010/byteact-reg-4.jpg)](//img.beamnote.com/2010/byteact-reg-4.jpg)

选择刚才第二步下载的 index.php, 完成之后点击对号.

[![byteact_reg5](//img.beamnote.com/2010/byteact-reg-5.jpg)](//img.beamnote.com/2010/byteact-reg-5.jpg)
> <span style="color: #888888;">_上传按钮具体形式与浏览器有关_</span>
现在访问你的域名, 应该转入 Google.com. 如果不成功, 请重试.

至此, 中转服务器搭建完成.

需要注意的是, 现在与 Opera Mini 服务器不是直接通讯, 大大增加了传输数据被窃听的可能性, 在浏览包含个人信息和敏感内容的网页时应特别小心.
