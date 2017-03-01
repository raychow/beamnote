---
title: 【纯探索向】获取访问者的天气
tags:
  - WordPress
  - 博客
  - 天气
id: 261
categories:
  - 技巧
date: 2010-10-17 22:37:18
---

之所以会弄出这个命题, 是突然之间的一个想法: 想让博客的背景随着访问者当地的天气变化而变化. 好天气, 微笑的晴天娃娃; 下雨天, 晶莹的水滴点点; 飘雪了, 唯美的白色浪漫.

[![获取访问者的天气](//img.beamnote.com/2010/get-the-weather-of-visitors.png)](//img.beamnote.com/2010/get-the-weather-of-visitors.png)<!-- more -->

虽然目的简单, 做起来缺有点绕路, 单从数据源来说, 各提供天气服务的网站都不会公开天气预报的接口, 只能在网路上搜寻大家撞见的地址.

## 一、腾讯网天气预报

腾讯有一个获取天气预报的脚本, 已经将函数封装好, 直接使用就能获得访问者当地的天气情况, 稍加修改就能实现目标.

> 获取最新脚本请[按此](http://minisite.qq.com/js/j.minisite.weather.js);
>
>
> 如果新版本不能使用, 获取本站存档请[按此](/wp-content/uploads/2010/10/j.minisite.weather.js).

如果有一个名为 `Weather` 的元素, 使用下面的代码就能将其内容替换为天气情况:

{% codeblock lang:html %}
<script src="j.minisite.weather.js"></script>
<script type="text/javascript">MiniSite.Weather.print('Weather');</script>
{% endcodeblock %}

这一函数完成了以下工作:

1. 读取名为 `QQ_IPAddress` 的 Cookie, 如果存在则载入保存的省名、城市名, 跳到第三步;
2. 访问 http://fw.qq.com:80/ipaddress 获得省名、城市名, 保存 Cookie;
3. 获得城市编号;
4. 通过 http://weather.news.qq.com/inc/minisite_[城市编号].js 获得天气字符串, 加入链接.

分析执行速度, 第一次执行发出两次请求, 第二次由于保存了 Cookie, 发出的请求减少为一次, 速度较好.

如果要实现天气背景, 应修改 j.minisite.weather.js, 使其直接返回天气而不是替换元素.

## 二、中国天气网

中国天气网的接口比腾讯的麻烦, 但是它给出的天气可以用数字表示, 这一点比腾讯方便.

这网站没有给我们留下直接使用的脚本, 还得自己开工, 需要完成以下工作:

1. 访问 http://61.4.185.48:81/g/, 得到城市编号;
2. 下载 http://m.weather.com.cn/data/[城市编号].html 的内容, 这是一段 JSON 数据;
3. 分析数据, 得到天气
由于涉及到跨站请求, 单纯的通过 JavaScript 是不能达成目标的, 必须使用 AJAX 中转, 或者纯 PHP 方式实现.

至于获得的 JSON 数据, 只要把它当作一段普通的字符串, 使用正则匹配:

* `/(\"img1\":\").*?([^\"]+)/i` 可以获得天气代码;
* `/(\"weather1\":\").*?([^\"]+)/i` 可以获得天气描述.

使用天气代码更方便, 因为它是一个范围从 0 到 31 的数字, 访问 http://m.weather.com.cn/weather_img/[天气代码].gif 即可获知实际含义.

分析执行速度, 第一次执行发出两次请求 (AJAX 还更慢), 可以将通过 Cookie 保存城市编号, 这样第二次只需执行后两步, 减少一次请求; 或者通过生命周期较短的 Cookie 保存天气, 因为天气背景不用那么实时 :-), 请求数为零.
