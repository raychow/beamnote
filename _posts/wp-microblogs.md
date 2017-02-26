---
title: "WordPress 微博插件: WP Microblogs"
tags:
  - WordPress
  - WP Microblogs
  - 微博
  - 插件
id: 339
categories:
  - 作品
date: 2011-02-11 19:51:43
---

> 由于学业繁忙, 此插件停止开发, 抱歉.

**抱歉, 由于新浪微博官方 API 修订, 此插件已失效. 详见 [http://beamnote.com/2012/stop-support-of-sina-weibo.html](http://beamnote.com/2012/stop-support-of-sina-weibo.html). **

[![WP Microblogs](http://img.beamnote.com/2011/wp-microblogs.jpg)](http://img.beamnote.com/2011/wp-microblogs.jpg)<!-- more -->

## 一、简介

WP Microblogs 可以在 WordPress 中显示最新微博, 目前支持新浪微博、腾讯微博、Twitter、网易微博、搜狐微博、饭否、豆瓣除 `XAuth` 之外的所有可用的认证方式对于更加开放的微博 (例如 Twitter、饭否), 只输入用户名即可展示微博.

0.4.0 版更新介绍: [http://beamnote.com/2012/wp-microblogs-0-4-0.html](http://beamnote.com/2012/wp-microblogs-0-4-0.html)

0.3.3 版更新介绍: [http://beamnote.com/2011/wp-microblogs-0-3-3.html ](http://beamnote.com/2011/wp-microblogs-0-3-3.html)

 (本页面将要更新, 请稍等)

在目前的版本中, 至少已经包含下列功能:

* 提供一种直接展示最新微博的小工具;
* 智能过滤重复微博, 为微博中提到的 URL 添加链接;
* 使用 `wm_tweet()` 或 `wm_tweets()` 在指定位置展示最新的一条或数条微博;
* 使用 `wm_get_tweet_arr()` 或 `wm_get_tweets_arr()` 获得微博原始数据;
* 较完善的缓存机制, 减少资源占用;
* 提供数个过滤器(filter)与动作(action)自定义展示方式.

有人问这与使用官方微博展示插件有何区别, 我这样回答:

* 可以按时间顺序同时显示多个微博, 还能过滤重复的微博;
* 样式可由自己控制, 想做成啥样就做成啥样, 例如给图片加上灯箱 (参见右方小工具「我在说啥」), 或者集成到一句话公告模块;
* 提供缓存机制, 不必担心微博网站挂掉, 国外主机还能直接显示 Twitter 时间线.

### 图片展示:

后台设置:

[![WordPress 微博 后台设置](http://img.beamnote.com/2011/wp-microblogs-1.png)](http://img.beamnote.com/2011/wp-microblogs-1.png)

小工具设置:

[![WordPress 微博 小工具设置](http://img.beamnote.com/2011/wp-microblogs-2.png)](http://img.beamnote.com/2011/wp-microblogs-2.png)

小工具输出:

[![WordPress 微博 小工具输出](http://img.beamnote.com/2011/wp-microblogs-3.png)](http://img.beamnote.com/2011/wp-microblogs-3.png)

## 二、安装与启用

1. WordPress 官方下载地址: [http://wordpress.org/extend/plugins/wp-microblogs/](http://wordpress.org/extend/plugins/wp-microblogs/) 或者在后台「添加新插件」面板搜索 WP Microblogs 直接安装;
2. 安装并启用插件后, 在设置菜单中找到 WP Microblogs 菜单项, 进入设置面板;
3. 添加微博帐号. 共有三种认证方式, 通常一种微博只支持部分认证方式:

    * 不认证: 输入用户名即可展示微博. 如果您的微博帐号设置了隐私保护而不能获得时间线, 这种认证方式将无法使用;
    * `OAuth`: 安全的 `OAuth` 认证, 用户名与密码在微博官方输入;
    * `Basic Auth`: 用户名与密码将保存在数据库中.
    * 所有的微博帐号都至少提供了前两种认证方式中的一种, 由程序默认列出的认证方式为可用的最安全方式;

4. 添加微博小工具. 在小工具面板中找到「微博」小工具, 拖动到右方启用.

## 三、简单设定

设定内容分为 WP Microblogs 面板的全局设置与每个小工具的局部设置.

### 1\. WP Microblogs 面板

* 更新频率: 设定微博更新间隔的时间, 最少为 1 分钟. 过于频繁的更新不仅没有必要, 还浪费了服务器资源;
* 每个帐户最多保存微博数量: 受于部分微博服务商的限制, 最多可以保存 20 条微博;
* 过滤重复微博: 如果您需要同时显示多个微博, 而这些微博中有部分内容来源于微博群发工具, 此功能可以过滤不同微博的相同发言(tweets);
* 使用近似匹配: 这个选项将忽略微博中提到的网址, 解决部分微博服务商自动缩短网址造成重复微博判断失误;
* 为微博中提到的 URL 添加链接: 链接中会自动包含 nofollow.

### 2\. 小工具设置

* 显示相对时间: 如果勾选, 今天发表的微博都将显示相对时间.

[高级应用: 在任何地方显示微博](http://beamnote.com/2011/wp-microblogs.html/2)

[高级应用: 使用自定义样式表与 JS 脚本](http://beamnote.com/2011/wp-microblogs.html/3)

<!--nextpage-->

## 四、高级应用

现在的版本我没有在这个插件的外观上下工夫, 不过预留了几个函数用以控制输出.

### 1\. 在任何地方显示微博

请在主题源文件中使用以下函数:

{% codeblock lang:php %}
wm_tweet( $args );      // 显示最新的一条微博
wm_get_tweet( $args );  // 以数组形式返回最新的一条微博
wm_tweets( $args );     // 显示最新的数条微博
wm_get_tweets( $args ); // 以数组形式返回最新的数条微博
{% endcodeblock %}

它们的参数与使用方法都是相同的.

#### 参数示例:

{% codeblock lang:php %}
<?php $args = array(
    'count' => 1,
    'list_wrapper' => '<div class="tweet">%s</div>',
    'tweet_wrapper' => '',
    'tweet_format' => '[name] : [text][pic][rt] <span class="meta">( <a href="[tweet_url]">[time]</a> 来自 <a href="[user_url]" rel="external nofollow">[type]</a> )</span>',
    'pic_format' => '',
    'rt_pic_format' => '',
    'rt_format' => ''
); ?>
{% endcodeblock %}

或

{% codeblock lang:php %}
<?php wm_tweet('count=1&amp;list_wrapper=<div class="tweet">%s</div>&amp;tweet_wrapper=&amp;tweet_format=[name] : [text][rt] <span class="meta">( <a href="[tweet_url]">[time]</a> 来自 <a href="[user_url]" rel="external nofollow">[type]</a> )</span>&amp;pic_format=&amp;rt_pic_format&amp;rt_format='); ?>
{% endcodeblock %}

#### 参数列表:

* mid
    微博的编号.
    编号为 WP Microblogs 控制板中微博列表每行第一个数字.
* count
    要返回微博的总数.
* list_wrapper
    包围微博列表 HTML 代码.
    例如: `<ul>%s</ul>`
    `%s` 是微博列表的占位符, 运行时将被微博列表替换.
* tweet_wrapper
    包围每条发言的代码.
    例如: `<li>%s</li>`
    `%s` 是微博发言的占位符, 运行时将被微博发言替换.
* tweet_format
    微博发言的 HTML 结构.
    例如: `[name] : [text][pic][rt] <span class="time">[time]</span>`
    中括号及括号包围的部分是预定义的占位符, 运行时将被实际内容替换, 例子在运行时可能成为这样:

    {% codeblock lang:html %}
    Ray Chow : 微博发言<div class="pic"><img src="http://img.beamnote.com/pic.jpg" /></div><div class="rt">转 User : 被转发言<div class="rt-pic"><img src="http://img.beamnote.com/rt-pic.jpg" /></div><span class="time">2012-12-21 00:00</span>
    {% endcodeblock %}

    详细的占位符定义请参见下一小节.
* pic_format
    发言中图片的 HTML 结构.
    例如: `<div><a href="[pic_big]" rel="external nofollow"><img src="[pic_small]" /></a></div>`
* rt\_pic\_format
    锐推中图片的 HTML 结构.
    例如: `<div><a href="[rt_pic_big]" rel="external nofollow"><img src="[rt_pic_small]" /></a></div>`
* rt_format
    锐推的 HTML 结构.
    例如: `<div><a href="[rt_user_url]" rel="external nofollow">[rt_name]</a> : [rt_text][rt_pic]</div>`
* time_format
    时间格式.
    例如: `n月j日 H:i`
    参见 [http://www.php.net/manual/zh/function.date.php](http://www.php.net/manual/zh/function.date.php).
* relative
    使用相对时间.
    设定为 1 则使用相对时间, 设定为 0 则使用绝对时间.

#### 占位符:

<table border="0">
<tbody>
<tr>
<td>[type]</td>
<td>微博服务商名称</td>
</tr>
<tr>
<td>[name]</td>
<td>用户昵称</td>
</tr>
<tr>
<td>[user_url]</td>
<td>用户 URL</td>
</tr>
<tr>
<td>[tweet_url]</td>
<td>微博发言 URL</td>
</tr>
<tr>
<td>[time]</td>
<td>发言时间</td>
</tr>
<tr>
<td>[text]</td>
<td>发言内容</td>
</tr>
<tr>
<td>[pic_small]</td>
<td>发言图片 (小) </td>
</tr>
<tr>
<td>[pic_big]</td>
<td>发言图片 (大) </td>
</tr>
<tr>
<td>[rt_name]</td>
<td>锐推微博的用户昵称</td>
</tr>
<tr>
<td>[rt_user_url]</td>
<td>锐推微博的用户 URL</td>
</tr>
<tr>
<td>[rt_tweet_url]</td>
<td>锐推微博的 URL</td>
</tr>
<tr>
<td>[rt_text]</td>
<td>锐推内容</td>
</tr>
<tr>
<td>[rt_pic_small]</td>
<td>锐推图片 (小) </td>
</tr>
<tr>
<td>[rt_pic_big]</td>
<td>锐推图片 (大) </td>
</tr>
</tbody>
</table>

以上占位符可用于 `tweet_format`、`pic_format`、`rt_format`、`rt_pic_format`.

<table border="0">
<tbody>
<tr>
<td>[pic]</td>
<td>发言图片 HTML 结构</td>
</tr>
</tbody>
</table>
此占位符将按照 `pic_format` 的格式替换, 可用于 `tweet_format`、`rt_format`、`rt_pic_format`.
<table border="0">
<tbody>
<tr>
<td>[rt_pic]</td>
<td>锐推微博图片 HTML 结构</td>
</tr>
</tbody>
</table>
此占位符将按照 `rt_pic_format` 的格式替换, 可用于 `tweet_format`、`rt_format`.
<table border="0">
<tbody>
<tr>
<td>[rt]</td>
<td>锐推 HTML 结构</td>
</tr>
</tbody>
</table>
此占位符将按照 `rt_format` 的格式替换, 可用于 `tweet_format`.

[![Tweet](http://img.beamnote.com/2011/tweet-structure.png)](http://img.beamnote.com/2011/tweet-structure.png)
<!--nextpage-->

### 2\. 使用自定义样式表与 JS 脚本

WP Microblogs 样式表位于 `/wp-microblogs/style.css` 文件. 尽管可以直接修改此文件, 我依然建议您使用下面这个方法, 即使插件升级, 您的修改也不会丢失.

建议您将经过修改的 WP Microblogs 样式表放置在主题目录下, 使用这段代码让 WordPress 放弃插件自带样式表而加载新的样式表. 这段代码应该放在主题的 functions.php 中.

{% codeblock lang:php %}
function customize_wm_style() {
    wp_deregister_style('wm');
    wp_register_style('wm', '/path/wp-microblogs.css');
}

add_action('wm_register_ss', 'customize_wm_style');
{% endcodeblock %}

请注意将例子中的路径用实际路径替换.

WP Microblogs 会在加载小工具时自动加载一个 JS 脚本实现上下滚动的效果. 如果您修改或者完全重写了这个脚本, 也建议您将经过修改的脚本放在主题目录下, 类似的, 使用下面这段代码加载自定义的脚本.

{% codeblock lang:php %}
function customize_wm_script() {
    wp_deregister_script('wm-widget');
    wp_register_script('wm-widget', '/path/wp-microblogs.js');
}

add_action('wm_register_ss', 'customize_wm_script');
{% endcodeblock %}

如果您在自定义的 JS 脚本中调用了 `jQuery`, 需要保证 `jQuery `库在 JS 脚本之前加载.

如果您的主题使用 WordPress 官方建议的方式加载 `jQuery` 库 (即使用了 `wp_enqueue_script` 函数), 或者没有加载, 可以使用下面的代码满足 `jQuery` 库优先加载的关系:

{% codeblock lang:php %}
function customize_wm_script() {
    wp_deregister_script('wm-widget');
    wp_register_script('wm-widget', '/path/wp-microblogs.js', array('jquery'));
}

add_action('wm_register_ss', 'customize_wm_script');
{% endcodeblock %}

如果主题不是以官方建议的方式加载了 `jQuery` 库, 不要使用上述方法, 否则可能因重复加载 `jQuery` 库导致冲突.

### 3\. 深入定制小工具

除了使用小工具面板提供的选项自定义之外, 还可以使用过滤器(filter)深入定制小工具.

#### filter 名称:

wm_widget_args

#### filter 用途:

小工具使用 `wm_get_tweets()` 函数输出时间线, 此过滤器可修改函数使用的参数.

#### filter 回调参数:

* str: 原始参数;
* mid: 微博编号;
* count: 显示的微博条数;
* relative: 是否显示相对时间.

#### 使用示例

{% codeblock lang:php %}
function customize_wm_widget($str, $mid, $count, $relative) {
    return "mid=$mid&amp;count=$count&amp;relative=$relative&amp;tweet_format=[text][pic][rt]<div class=\"meta\"><a href=\"[tweet_url]\" rel=\"external nofollow\">[time]</a> 来自 <a href=\"[user_url]\" rel=\"external nofollow\"><img src=\"[user_head]\" /></a></div>";
}

add_filter('wm_widget_args', 'customize_wm_widget', 10, 4);
{% endcodeblock %}

这段代码应该放在主题的 functions.php 中. 执行这个例子后, 输出的来源部分将变为您的头像.
