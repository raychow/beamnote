---
title: 讨论获得「时间范围」
tags:
  - WordPress
  - 博客
  - 时间
  - 读者墙
id: 284
categories:
  - 技巧
date: 2011-01-14 08:18:25
---

> 题外话: 最近在做一个 WordPress 插件, 用于支持各种微博在 WordPress 上的显示, 您可以使用此插件
>
> * 显示所有微博中的最新的一条发言 (例如显示 Twitter、新浪微博中最新的一条微博);
> * 按时间顺序显示数条各种微博的发言 (如果您各微博内容不同).
> 欢迎针对此插件进行讨论 :-).

WordPress 的读者墙应该是讨论到烂掉的问题了, 这里肯定不会说有关读者墙的实现 (我现在的读者墙是通过模板实现的, 更省资源), 咱们来看看时间范围的选择.

[![](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2011/time-range.jpg)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2011/time-range.jpg)<!-- more -->

说起「一个月」的时间, 各位可能有两种反应, 一是「从上个月的本日直到今天, 大约三十天的时间」, 另一个是「从本月初到今天的时间」; 读者墙统计的时间范围也因此有两种可以选择, 相应的, 「从前 ×× 天 / 周 / 年直到今天的时间」与「从本周 / 月 / 季 / 年初到今天的时间」, 具体实现方法也不相同.

## 一、PHP 时间函数的一点问题

`strtotime` 函数可以用形象的方式获得相对时间, 但这个函数可能返回你没有期望的值.

例如, 在2011 年 3 月 31 日这一天, 您认为 `strtotime( '-1 Month' )` (字面意义为向前一个月) 的结果是哪天? 由于 2 月没有 31 天, 那么会是 2 月 28 日, 还是 3 月 1 日? 函数告诉你, 是 3 月 3 日, 我猜 PHP 是这样认为的: 3 月 31 日前一月为 2 月 31 日, 但 2 月只有 28 天, 那么就再向后延 3 天, 为 3 月 3 日.

事实上, 「前一个月」本就是笼统的说法, 因为每个月的天数都不相同, 还存在上例中本月的日子在上月不存在的情况, 有的时候, 用「前 30 天」代替「前一个月」更为准确.

不过呢, 读者墙也不是特别重要的东西, 只是如果有比较严谨的场合就要注意啦.

## 二、统计从前××天 / 周 / 月 / 年直到今天

只写出开始统计的日期.
{% codeblock lang:php %}
$range_start = date( 'Y-m-d H:i:s', strtotime( '-1 day' ) );   // 前 1 天
$range_start = date( 'Y-m-d H:i:s', strtotime( '-1 week' ) );  // 前 1 周
$range_start = date( 'Y-m-d H:i:s', strtotime( '-30 days' ) ); // 前 1 月
$range_start = date( 'Y-m-d H:i:s', strtotime( '-1 year' ) );  // 前 1 年
{% endcodeblock %}

## 三、统计从本周 / 月 / 季 / 年初直到今天

`strtotime` 是无法直接反映本季初的概念的, 因此需要使用 `mktime` 函数分开年月日进行计算.

使用 `strtotime` 可以表示本周 / 月 / 年初.
{% codeblock lang:php %}
$range_start = date( 'Y-m-d H:i:s', strtotime( 'this week' ) );                // 本周初
$range_start = date( 'Y-m-d H:i:s', strtotime( 'first day of this month' ) );  // 本月初
$range_start = date( 'Y-m-d H:i:s', strtotime( 'first day of this year' ) );   // 本年初
{% endcodeblock %}
使用 `mktime `则略显麻烦.
{% codeblock lang:php %}
$range_start = date( 'Y-m-d H:i:s', mktime( 0, 0, 0, date( 'n' ), date( 'd' ) - ( date( 'w' ) == 0 ? 7 : date( 'w' ) ) + 1, date( 'Y' ) ) );    // 本周初
$range_start = date( 'Y-m-d H:i:s', mktime( 0, 0, 0, date( 'n' ), 1, date( 'Y' ) ) );                                                           // 本月初
$range_start = date( 'Y-m-d H:i:s', mktime( 0, 0, 0, date( 'n' ) - ( date( 'n' ) - 1 ) % 3, 1, date( 'Y' ) ) );                                 // 本季初
$range_start = date( 'Y-m-d H:i:s', mktime( 0, 0, 0, 1, 1, date( 'Y' ) ) );                                                                     // 本年初
{% endcodeblock %}
