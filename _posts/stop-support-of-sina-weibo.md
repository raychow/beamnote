---
title: 微博插件将停止对新浪微博的支持
tags:
  - WP Microblogs
id: 425
categories:
  - 唠叨
date: 2012-08-27 13:30:47
---

很遗憾的通知各位, 微博插件 WP Microblogs 近期将停止对新浪微博的支持, 准确地说, 是被迫停止支持.

新浪微博于近期全面启用了 OAuth 2.0 授权, 本插件仅支持旧版 OAuth, 因此在月底左右将无法通过认证, 亦无法获取微博.

停止支持的决定出于以下原因而作出:

1. 最主要的原因, 是 (新浪微博的) OAuth 2.0 授权要求每个 Key 精确的指定「授权回调页」与「取消授权回调页」, 工作在不同网站的本插件显然无法满足这一要求;
2. 新浪微博要求定期更新一次授权, 十分麻烦, 反倒不如使用官方 Widget 挂件;
3. 尽管可以通过一个中转页面代理实现授权, 但我几乎没有空余时间用于开发这款插件; 况且新浪不一定允许这样的行为.

如果有其它微博也作出如此无语的升级, 此插件基本上会停止支持.
