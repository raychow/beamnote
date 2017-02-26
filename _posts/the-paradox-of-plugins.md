---
title: "WordPress: 插件与非插件的悖论"
tags:
  - WordPress
  - 博客
  - 插件
id: 258
categories:
  - 分享
date: 2010-10-12 00:31:09
---

还在几年之前, 做网站是一件比较麻烦的事情, 可能一个站点从上至下都是程序员手工码出的代码. 在网站建成之后还需要维护, 如果连添加新文章都要进行系统培训那还怎么交付客户使用呢? 于是后台诞生了.

自从有了后台, 发新闻、删贴删评论的事情大家都会做了. 不久之后, 网站需要添加新功能, 例如敏感词系统, 这样审查员们的工作量又能减少. 由于很多网站都觉得敏感词的确是个好东西, 纷纷拜托程序员为自己的网站程序添加敏感词功能, 程序员被烦的多了, 心想为什么不开发一种机制让这些后来可能添加的功能像插座那样即插即用呢? 于是插件诞生了.

[![WordPress: 插件与非插件的悖论](http://img.beamnote.com/2010/the-paradox-of-plugins.png)](http://img.beamnote.com/2010/the-paradox-of-plugins.png)<!-- more -->

现在, 如果工信部发出通知, 要求所有留言者必须填写网民证号才能留言, 程序员只需为每种程序开发一套用于验证的插件就可以让千千万万采用该程序的网站满足工信部的规定, 世界和谐了.

具体到 WordPress, 插件玲琅满目数量可观, 但当博客添加的插件越来越多, 网站拥有者觉得这些插件实现与直接修改源程序相同的功能, 却占用更多的资源, 纷纷表示不划算, 要求将插件整合到源码里去, 一篇篇「非插件实现×××」的文章应运而生.

周而复始的工作.

尽管实现一些简单功能的 WordPress 插件源代码并不复杂, 但考虑到程序的可靠性、适应性、健壮性, 多数插件需要额外的代码来达到一个「足够好」的程序的标准. 例如, 用于增强留言体验的插件需要考虑到用户模板不标准的情况 (例如 WP Thread Comment), 用于生成站点地图的插件需要顾及内存限制、服务器组件缺失的状况 (例如 Google XML Sitemaps), 而几乎稍微复杂一些的插件在使用部分 WordPress 函数时都需要判断是否被定义, 因为它们可能在当前版本的 WordPress 中尚未定义或已被弃用.

有些程序为了加强结构逻辑、增强复用性和扩展性, 也为了更简洁、易于维护, 采用了面向对象的架构, 但 OOP 也导致了代码增多、运行效率降低.

就 WordPress 插件工作机制而言, 让插件工作需要经历以下步骤 (详见[《深入剖析 WordPress 插件机制》](http://www.duduwolf.com/wiki/2007/353.html)) :

1. 遍历 plugins 文件夹, 得到此文件夹下所有 PHP 文件, 并通过 `get_plugin_data` 找到插件的描述信息;
2. 激活插件. 此步骤只需要写入数据库;
3. 通过 wp-config.php 中的引用, 在每个页面加载时 Include 所有被激活的插件;
4. 在 Include 插件时, WordPress 会通过 `add_action`、`apply_filters`、`add_filter`、`do_action` 把程序定义的函数挂载到扩展点中; 当扩展点执行时, 系统会执行所有被挂载的函数.

由上面的分析, 可以看出 WordPress 插件工作占用的资源的确要比直接修改源代码更多, 这就有一点如开篇所述的悖论:

为了方便而用插件 → 占资源 → 为了省资源而改源码 → 不方便 → 为了方便而用插件 → ……

有人主张把插件的源码提取出来放到主题的 functions.php 中, 我认为这并不能提高多少效率. 从上面的四个步骤来看, 尽管省却了前两步的时间, 但这两步仅在进入 WordPress 控制版与管理插件时发生, 真正占用时间的是第四步, 即使把源码放在 functions.php 中, 只要保留了钩子, 效率就不会提高.

对于这个悖论, 我个人的看法是:

* 功能简单、不使用钩子或使用钩子少的插件整合到主题中, 只保留适合当前 WordPress 环境的部分, 去除诸如版本、主机环境判断的部分;
* 功能复杂、使用钩子多的插件没有使用「非插件实现」的必要, 直接使用即可, 否则 WordPress 的插件机制留着做什么?
* 就你的博客而言, 访问量真的达到需要通过减少插件数量降低服务器负载的地步了吗?