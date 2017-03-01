---
title: YOURLS 加强版
tags:
  - YOURLS
  - 网络
id: 208
categories:
  - 作品
date: 2010-07-27 17:47:06
---

网址缩短站 2cm.in 弄得我很无语, 因为总是有人用它缩短钓鱼网站, 删了一次又一次, 可惜 YOURLS 没有黑名单功能, 防不胜防. 实在没辙了, 只好暂停 2cm.in 的服务, 想找一个可以实现屏蔽功能的程序, 所以才有了《关于重置 2cm.in 网址缩短服务的通知》.

昨天在网路搜寻了许久, 试用了多款网址缩短程序, 只发现 urlShort 这一款能够连接 PhishTank 识别钓鱼网站之外, 都没有任何防护措施. 而 urlShort 没有控制台, 想检视已缩短的网站都很不方便. 看来没有可以屏蔽网址的缩短程序, 干脆自己动手打造 YOURLS, 实现我的需求.

[![YOURLS 加强版](//img.beamnote.com/2010/yourls-mbrc.png)](//img.beamnote.com/2010/yourls-mbrc.png)<!-- more -->

YOURLS 加强版具有以下特点:

* 封锁 IP
* 封锁单个网址
* 封锁整个域名
* 通过 PhishTank 判断并阻止钓鱼网站
* 一键创建黑名单数据表

尽管加入了这些功能, YOURLS 加强版并没有改变原版的使用习惯, 下面是实际运行效果图:

[![YOURLS 加强版](//img.beamnote.com/2010/2010-07-27_16-48-11.png)](//img.beamnote.com/2010/2010-07-27_16-48-11.png)

**在开始使用之前, 请先单击网页下方的「创建表」按钮, 自动在 YOURLS 数据库中创建黑名单表. **

## 详细功能介绍

### 自动封锁

动作区域比原版增加了后面三个按钮, 通过单击这三个按钮, 可以直接封锁此行 IP、网址或域名.

封锁域名精确到每个子域名, 例如: 将 baidu.com 域名封锁, 那么 www.baidu.com/more/ 将被自动封锁, 但 tieba.baidu.com 不在封锁之列.

[![YOURLS 加强版](//img.beamnote.com/2010/2010-07-27_17-00-42.png)](//img.beamnote.com/2010/2010-07-27_17-00-42.png)

### 手工封锁

如果要封锁未被列出的目标, 可以使用网页下方的手工添加封锁功能.

[![YOURLS 加强版](//img.beamnote.com/2010/2010-07-27_17-13-09.png)](//img.beamnote.com/2010/2010-07-27_17-13-09.png)

请注意, 管理功能尚未开发, 因此如果需要修改 / 删除封锁项目, 请在 phyMyAdmin 中进行, 黑名单位于 yourls 数据库 / yourls_blacklist.

### PhishTank

PhishTank 是基于社区的反钓鱼式攻击服务.

PhishTank 于 2006 年 10 月 2 日作为 OpenDNS 的子公司建立, 用户可以从世界各地向其汇报钓鱼网站, 经其他用户以投票的形式认证后, 即通过公开的 API 共享给所有使用 PhishTank 服务的机构和个人.

YOURLS 加强版内置了 PhishTank 反钓鱼功能, 官方建议您编辑 includes/config.php, 在 YOURLS_PTKEY 字段填入 PhishTank API. 您可以在[这里](http://phishtank.com/api_register.php)申请 API.

当收到网址缩短请求时, YOURLS 首先会将网址传送给 PhishTank, 如果确定是钓鱼网站, 则拒绝服务并给出提示信息:

[![YOURLS 加强版](//img.beamnote.com/2010/2010-07-27_17-32-31.png)](//img.beamnote.com/2010/2010-07-27_17-32-31.png)

### 期待加入的功能

* 黑名单管理
* 更多防钓鱼服务

> 按这里下载: [本地下载](http://raychow.info/wp-content/uploads/2010/07/yourls-1.4.3-mbrc-1.0.zip), [uushare](http://www.uushare.com/user/raychow/file/3326812)
