---
title: 与时俱进的电脑软件大搜集 (二)
tags:
  - 安全
  - 电脑
  - 软件
id: 430
categories:
  - 技巧
date: 2012-11-18 00:06:58
---

上一话是开端, 肯定有一些人觉得介绍的软件不是那么的新奇异缺; 嘛, 毕竟这里标题的是「与时俱进」, 第一强调用新的更好用的软件代替掉旧软件, 第二强调用树立使用免费软件或正版正版的观念, 最后则是这里的软件都是我试用之后觉得不错的诚意推荐. 就是这样, 「与时俱进」的电脑软件大搜集第二话在时隔一月之后再次开篇 (～好慢\! ～).

[![与时俱进的电脑软件大搜集 (二)](http://img.beamnote.com/2012/computer-software-collection-2.jpg)](http://img.beamnote.com/2012/computer-software-collection-2.jpg)<!-- more -->

今次要推荐的软件是适合于普通电脑的两款免费安全软件 (言下之意, 以不折腾但又勘用为主), 德国 Avira Operations GmbH &amp; Co. KG 开发的杀毒软件 Avira Free Antivirus, 与美国 PWI, Inc 开发的防火墙 Privatefirewall.

Avira 在中国已经是比较知名的杀毒软件了, 中国网民亲切的称呼其为「小红伞」. Avira 向普通用户提供免费版 (Avira  Free Antivirus) 、高级版 (Avira Antivirus Premium) 与互联网安全版 (Avira Internet Security) 这几个不同的版本, 显而易见, 高级版与互联网安全版是收费的 (且官方收费昂贵, 如果需要购买, 建议先看看经官方授权的淘宝店).

免费版只提供较为单纯的病毒防护功能 (现在还提供了 Web Protection 功能, 但使用此功能需要额外安装浏览器插件);

高级版不仅包括免费版的全部功能, 还提供了 Web Protection 与 Avria 专家的实时支持 (如上所述, 免费版已经拥有了 Web Protection, 而对于个人用户专家支持也显得比较鸡肋, 所以这个版本的存在意义是什么? );

互联网安全版不仅包括高级版的的全部功能, 还提供了防火墙、垃圾邮件防护与备份工具 (垃圾邮件防护交给邮件服务器做就好了, 备份工具显然与 Windows 自带功能重复…).

所以现在针对不折腾的个人用户推荐两种方案, Avira 免费版 + Privatefirewall, 或是 Avira 互联网安全版.

## 方案一: Avira 免费版 + Privatefirewall

对于第一种方案, 由上所述 Avira 最受欢迎的其实是免费版, 不仅因为免费, 还因为这个版本提供了一个单纯的防病毒软件——[病毒查杀能力高](http://www.virusbtn.com/vb100/archive/test?recent=1)、没有防火墙、没有太多的主动防御、较低的硬件要求, 很适合与其它安全软件搭配使用.

[![Avira](http://img.beamnote.com/2012/avira_free.png)](http://img.beamnote.com/2012/avira_free.png)

卡饭论坛上流行的搭配组合是「红豆」组合, 即将 Comodo 防火墙与 Avira 免费版进行搭配. Comodo 亦是一款来自美国的安全产品, 其防火墙也带有 HIPS, 只是需要花费一些精力学习; 如果想要学习 Comodo, 请按[此处](http://bbs.kafan.cn/thread-1265632-1-1.html).

本文不打算讨论 Comodo 的更多内容; 对于普通用户来说, Privatefirewall 既可以不加设定的使用, 也可以手动打磨, 打磨的难度是低于 Comodo 的.

首先[下载](http://www.avira.com/zh-cn/download-start/product/avira-free-antivirus)安装 Avira 免费版. 请注意, Avira 的 Web Protection 的安装选项在勾选安装 IE 浏览器插件后才会出现, 但 Web Protection 与 Privatefirewall 有冲突, 会导致 Privatefirewall 不能拦截某些程序的流量. 尚不清楚各自的新版本是否解决了这个问题. 因为 Web Protection 表现的不是特别出色, 常有人反馈它拖慢了某些下载软件 (如迅雷) 的速度, 或者是因为误报导致少数网页显示异常. 由于 Web Protection 主要检测网络流量中的恶意内容, 但不具备防火墙的能力, 只安装病毒防护功能其实不会牺牲此方案的安全性.

另外, 即使是目前最新的版本 Avira 2013 也未能兼容 Windows8, 请 Windows8 用户慎重使用. 新的测试版 Avira Version 2013 Patch5 声称改善了 Windows8 兼容性, 可以按此[处获](https://betacenter.avira.com/project/version/default.html?cap=A788D05206E74FEC88A6C705F84F23E7&amp;arttypeid={B10B374A-D133-4469-B0D2-C44054D00026})得安装文件.

Avira 免费版带有广告, 基本上每天会弹出一次; 广告内容倒不像国产软件那般低俗, 多数为自家软件的宣传. 不过可以通过很简单的方式屏蔽广告, 具体方法请按[此处](http://bbs.kafan.cn/thread-1237447-1-1.html).

如果你打算试用 Web Protection, 修改注册表可以骗过 Avira 安装程序让你不安装浏览器插件就能使用 Web Protection (再次说明, Web Protection 可能导致 Privatefirewall 冲突) :

对于 32 位系统, 将以下文字保存为 reg 文件并双击导入注册表:

```
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SOFTWARE\AskToolbar]
[HKEY_LOCAL_MACHINE\SOFTWARE\AskToolbar\Macro]
"tb"="AVR-W1"
```

对于 64 位系统, 将以下文字保存为 reg 文件并双击导入注册表:

```
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\AskToolbar]
[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\AskToolbar\Macro]
"tb"="AVR-W1"
```

安装时取消勾选安装浏览器插件, 完成后会发现 Web Protection 成功开启.

再说 Privatefirewall. Privatefirewall 原来是一款付费软件, 但也许公司不做个人的生意了, 所以现在完全免费.

Privatefirewall 体积很小, 占用资源也很低, 界面更加朴素, 属于大部分时间都没有存在感的软件; 但倘若有什么可疑进程有动作, 它内建的 HIPS 就会发出警告.

[![Privatefirewall](http://img.beamnote.com/2012/privatefirewall_1.png)](http://img.beamnote.com/2012/privatefirewall_1.png)

中上方的三个图标为情景模式选择, 可以根据家庭 / 工作 / 公用网路选择不同的防护等级, Firewall 红绿灯可以关闭 / 打开防火墙, 或者完全阻止网路流量.

可以针对不同进程设定是否允许特定 IP 段 / 端口的入站 / 出站 UDP 或 TCP 访问:

[![Add / Edit Rule](http://img.beamnote.com/2012/privatefirewall_8.png)](http://img.beamnote.com/2012/privatefirewall_8.png)

Privatefirewall 还拥有一个轻量级 HIPS, 默认情况下会放行受信任的发布商所发行的程序, 而提醒未知程序的关键动作:

[![Process Moniter](http://img.beamnote.com/2012/privatefirewall_10.png)](http://img.beamnote.com/2012/privatefirewall_10.png)

如果信任此程序, 则同时勾选「Remember this setting」和「Apply to all alerts」并点击「Allow」, 否则根据情况暂时放行或阻止此动作, 甚至终止程序.

此外, 程序还提供了防火墙日志和当前监听网络的程序列表, 方便寻找网络异常源头.

Privatefirewall 拥有定期学习的功能, 会自动放行你常用的程序, 并结合其「默认放行受信任的程序」之功能, 既保护了你的电脑, 又不用进行繁琐的设定.

## 方案二: Avira 互联网安全版

方案一是完全免费且比较轻松的一种搭配, 但如果想要更加简单易用, Avira 互联网安全版也是不错的选择.

前面说过, Avira 互联网安全版除了病毒防护, 对个人用户意义较大的功能还有 Web Protection 与防火墙, 这些都不用特别设定就能使用.

[![Avira](http://img.beamnote.com/2012/avira_is_1.png)](http://img.beamnote.com/2012/avira_is_1.png)

Avira 不提供控制其它程序行为的 HIPS, 防火墙是纯粹的网路防火墙. 有新程序试图访问网路时会发出提示.

[![Avira](http://img.beamnote.com/2012/avira_is_2.png)](http://img.beamnote.com/2012/avira_is_2.png)

要详细设定防火墙的话也可以, 只是有些麻烦, 因为 Avira 提供的规则设定不太友好, 要在一段话里通过点击关键词来改变含义 (简直是奇葩了喂！) \! 点多几下头脑就会晕掉…

[![Avira](http://img.beamnote.com/2012/avira_is_3.png)](http://img.beamnote.com/2012/avira_is_3.png)

<del datetime="2015-03-01T18:26:53+00:00">Avira 互联网安全版是收费软件, 官方购买要价 RMB 292.00 / 年, RMB 585.00 / 三年, 但经官方授权的淘宝店却要便宜很多. </del>

<del datetime="2015-03-01T18:26:53+00:00">Safebeta 是 Avira 初进大陆市场时的合作店家, 现在似乎因为没时间打理的关系, Avira 互联网安全版正以 RMB 25.00 / 两年的价格抛售, 而且更有新浪微博转发送序列号的活动, 有需要的可以看看: </del>

<del datetime="2015-03-01T18:26:53+00:00">【正版特价】小红伞 Avira Internet Security 2012 2013 两年正版</del>

<del datetime="2015-03-01T18:26:53+00:00">请注意, 在上面这家店购买的序列号授权的用户为 Uschi Lotz 而非你的名字, 经过实验序列号也不可以叠加, 对此介意的朋友可以看看下面这家. </del>

<del datetime="2015-03-01T18:26:53+00:00">iholy 的店主似乎一直混迹于卡饭, 也是可以相信的店家, 价格要贵一些, RMB 234.00 / 三年: </del>

<del datetime="2015-03-01T18:26:53+00:00">【正版】Avira Internet Security2013 小红伞 三年 (S) </del>

发淘宝的链接不是为了做广告, 也不是为了什么返利, 只是提供更优惠的正版, 也方便大家对价格进行比较.
