---
title: WordPress 时间观
tags:
  - WordPress
  - 博客
  - 时间
id: 334
categories:
  - 技巧
date: 2011-02-01 13:37:30
---

> 题外话: WordPress 微博插件基本完工, 只差一点点展示设计. 目前支持新浪微博、腾讯微博、Twitter (暂不支持 API) 、网易微博、搜狐微博、嘀咕、饭否、做啥、人间除 XAuth 之外的所有可用的认证方式, 不支持 XAuth 是因为 OAuth 更安全. 对于更加开放的微博 (例如 Twitter、嘀咕、饭否、做啥), 支持只输入用户名即可展示微博. 此插件暂名 <span style="color: #000000;">WordPress Microblogs</span>, 欢迎起名与建议.
> 新年快到了, 大家都在写年终篇, 我也在此预祝各位春节快乐, 大展宏兔.
既然是新年, 那就再说说 WordPress 时间的事儿. 这两天写插件, WordPress 的时间观念给我开了一个小玩笑, 搜了一下好像没啥人说这事儿的, 先记录下来.

[![2011 年春节](http://img.beamnote.com/2011/2011-spring.jpg)](http://img.beamnote.com/2011/2011-spring.jpg)<!-- more -->

## 一、WordPress 「破坏」了PHP 的时区设置

不知你是否注意到了, 在 WordPress 环境下, 任何 PHP 时间函数, 例如 `date()` 返回的时间不正确. 第一次发现这情况, 我以为是 `php.ini` 设置问题, 检查后没有发现问题. 实际情况是, WordPress 会将时区设置为 UTC, 比咱们东八区早八个小时 (但愿你没忘记初中地理 :-) ), 在 `/wp-settings.php` 里有这样一句:

{% codeblock lang:php %}
if ( function_exists( 'date_default_timezone_set' ) )
    date_default_timezone_set( 'UTC' );
{% endcodeblock %}

而 WordPress 的基础配置文件 `/wp-config.php` 的结尾会载入这个文件, 因此只要笼罩在 WordPress 阴影下的语句都不能返回正确时间. 所以不得不使用以下写法:

{% codeblock lang:php %}
date_i18n('Y-m-d h:i:s');                   // 返回当地时间
current_time('timestamp');                  // 返回当地时间的 Unix 时间戳
current_time('mysql');                      // 返回适用于 MySQL 的时间格式
time() + get_option('gmt_offset') * 3600;   // 手工获得当地时间的 Unix 时间戳
{% endcodeblock %}

因此, WordPress 中能否获得正确时间与 PHP 的时区设置没有关系, 只受 WordPress 设置的影响 (于常规选项中).

## 二、以静制动的 WordPress Cron

在开发 Magic Links 插件时我就用过 WordPress 的 `Cron` 定时任务, 加入任务队列中的 action 会按设定的时间触发. 之前写过一篇文章: [《使用 Cron jobs 自动备份网站 (一) 》](http://beamnote.com/2010/cpanel-automatic-backup-chapter-1.html), Linux 中有专门为定时任务服务的程序, 你设定什么时间, 就会在那个时间触发.

WordPress 是运行于 PHP 的程序, PHP 本身好像没有提供定时任务的功能, 网友们给出的方法大致是: 在一个无限循环中, 使用 `sleep()` 让程序等待, 并使用 `set_time_limit(0)` 防止程序执行超时. 个人感觉这方法有点不保险, 有那么一丁点执行出错或者时间不准的可能.

探索了一下源码, WordPress 的定时任务, 实际上是被动式的, 在 `wp_options` 数据表中有一名称为 `cron` 的记录, 可以看出这是一个多维数组, 其中包含了定时任务队列的信息, 例如:

{% codeblock lang:php %}
Array
(
    [1296503945] => Array                                           // 下次发生时间的时间戳
        (
            [wm_update_timeline] => Array                           // 任务名称
                (
                    [40cd750bba9870f18aada2478b24840a] => Array     // 参数数组 args 的 MD5 编码
                        (
                            [schedule] => wm_update_schedule        // 时间计划名称
                            [args] => Array                         // 传递给函数的参数数组
                                (
                                )
                            [interval] => 60                        // 时间间隔
                        )
                )
        )
    [version] => 2                                                  // Cron 的版本号, 用于系统升级判断
)
{% endcodeblock %}

从中也可以看出, 对于同一个 `action`, 不同的参数也会被看作不同的 `Cron`.

WordPress 触发 `Cron` 的一般顺序为:

1. 使用 `wp_schedule_event()` 登记 `Cron`;
2. 载入 `/wp-config.php`, 此文件自动载入 `/wp-settings.php`;
3. `/wp-settings.php` 中载入 `/wp-includes/cron.php`、`/wp-includes/default-filters.php`;
4. `/wp-includes/default-filters.php` 中为 `sanitize_comment_cookies` 动作挂接一个函数: `add_action( 'sanitize_comment_cookies', 'wp_cron' )`;
5. `/wp-settings.php` 中执行: `do_action( 'sanitize_comment_cookies' )`, `/wp-includes/cron.php` 中的函数 `wp_cron()` 被触发;
6. 检查是否有 `Cron` 的发生时间到达或者超过了当前时间, 如果有, 则以异步的方式 (通过 HTTP 请求) 执行 `Cron` 指定的函数.

结论就是, 访问者每刷新一次页面, 才会进行 `Cron` 的检查, 也就是说如果一直没有人访问, `Cron` 将不会触发. 因为是异步加载的, 所以不用担心 `Cron` 阻碍显示页面.
