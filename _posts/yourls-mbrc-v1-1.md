---
title: YOURLS 加强版 v1.1
tags:
  - YOURLS
  - 网络
id: 210
categories:
  - 作品
date: 2010-07-29 21:19:09
---

咱劳动人民开发速度就是快\! 经过一整天的努力奋斗, YOURLS 加强版 v1.1 终于问世了, 更新的内容有:

* 完善的黑名单管理
* 一键升级
* 代码优化

[![YOURLS 加强版 v1.1](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/yourls-mbrc-v1-1.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/yourls-mbrc-v1-1.png)<!-- more -->

## 升级方法

对于已经安装了 v1.0 的同学, 您只需将文件全部解压, 并进入控制版单击「创建或升级表」即可自动升级. **建议在升级前备份数据库. **

请前天稍早时候下载 v1.0 的同学注意, 你们需要进入 phpMyAdmin 检查 yourls_blacklist 是否存在 class 字段, 如果存在, 请将该字段改名为 type.

## 更新介绍

### 黑名单管理

虽然 YOURLS 有了黑名单功能, 但缺少一个管理程序还真是费劲呢\! 想看看谁被 Block 了还得进入 phpMyAdmin, 万一误操作可就杯具了.

现在, YOURLS 加强版加入了黑名单管理功能, 进入控制版单击「管理」就可以进入:

[![YOURLS 加强版 v1.1](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/2010-07-29_20-53-07.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/2010-07-29_20-53-07.png)

进入时会检查版本喔\! 如果收到初始化错误的讯息, 说明您忘记升级黑名单数据表了.

由于黑名单中的时间在这次升级后才加入, 因此之前添加进黑名单的项目只能统一显示为 1970 年 1 月 1 日.

您将发现黑名单管理界面和控制版是一致的 (花了我大量时间分析代码 :-( ) :

[![YOURLS 加强版 v1.1](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/2010-07-29_21-03-04.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/2010-07-29_21-03-04.png)

操作方法与控制版相同, 不再赘述.

### 一键升级

如果不乱改数据库, 一键升级是没有问题的, 如前所述单击「创建或升级表」即可完成.
> 按这里下载: [本地下载](http://raychow.info/wp-content/uploads/2010/07/yourls-1.4.3-mbrc-1.1.zip), [uushare](http://www.uushare.com/user/raychow/file/3330552)
