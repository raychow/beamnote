---
title: 异步执行的 Gravatar 缓存
tags:
  - Gravatar
  - 博客
id: 240
categories:
  - 技巧
date: 2010-08-23 20:14:50
---

话说鄙人超级喜欢跟风, 谁写出一部技巧集, 我就能山寨再加以修改写出第二部. 把 Gravatar 头像缓存到本地服务器是很多博客会做的事情, 那是因为国内糟糕的网路环境访问 Gravatar 网站特别慢, 某些地区甚至与 Gravatar 绝缘.

[![异步执行的 Gravatar 缓存](//img.beamnote.com/2010/asynchronous-implementation-of-the-gravatar-cache.png)](//img.beamnote.com/2010/asynchronous-implementation-of-the-gravatar-cache.png)<!-- more -->

不少人用的是 Willin Kan 的[缓存 Gravatar 头像](http://kan.willin.org/?p=1320)之方法, 我昨天在本地调试主题的时候加上了这段代码, 但发现了一个比较严重的问题: 评论列表要等很长的时间才能显示, 甚至直接 30 秒超时. 原因是原代码中这样一句 (第 13 行) :

{% codeblock lang:php %}
copy($g, $e);
{% endcodeblock %}

copy 函数起到了把 Gravatar 头像复制到本地的作用, 此时 PHP 解释器只能等待复制完成才能继续运行, 反而成为拖慢速度的隐患.

本文适用于以下情况:

* 使用国内服务器但想缓存 Gravatar 头像;
* Gravatar 服务器出现问题时.

国外服务器由于访问 Gravatar 服务器很快, 因此不会有明显的延迟感, 但当以上情况时延迟就非常明显了.

该怎么解决这个问题呢? 某个 Gravatar 头像第一次被显示时, 实际上仍然是调用 Gravatar 服务器的资源, 与此同时博客的服务器再从 Gravatar 服务器将头像下载下来. 博客服务器下载头像的过程对用户访问是没有任何意义的, 应该将 copy 过程放在另一个线程执行, 评论列表输出不受影响, 问题就解决了.

PHP 本身是不支持多线程的, 我们可以将下载头像的代码写在一个单独的 PHP 文件中, 然后由处理评论列表的函数发起一个到此文件的请求, 并立即关闭连接, 继续向下执行. 被请求的文件进行下载工作就相当于异步执行了.

下面给出具体实行方法.

## 一、建立异步执行的 PHP 文件

在 WordPress 根目录或者其它文件夹新建 gravatar.php (也可以改成您喜欢的名字), 内容如下:

{% codeblock lang:php %}
if ( $_SERVER['REQUEST_METHOD'] != 'POST' ) {
    header( 'Allow: POST' );
    header( 'HTTP/1.1 405 Method Not Allowed' );
    header( 'Content-Type: text/plain' );
    exit;
}
$key = '123456'; // $key 为防止恶意提交的验证码
if ( !isset( $_POST['hash'], $_POST['s'], $_POST['key'] ) || $_POST['key'] != $key ) {
    header( 'HTTP/1.1 400 Bad Request' );
    header( 'Content-Type: text/plain' );
    exit;
}
$hash = $_POST['hash'];
if ( !ctype_alnum( $hash ) || strlen( $hash ) != 32 )
    return;
$s = strtoupper($_POST['s']);
if ( !ctype_digit( $s ) ) return;
$r = isset( $_POST['r'] ) ? $_POST['r'] : '';
$rating = array( '', 'G', 'PG', 'R', 'X' );
if ( !in_array( $r, $rating ) )
    return;
if ( defined( 'ABSPATH' ) )
    require_once( ABSPATH . 'wp-load.php' );
else
    require_once( './wp-load.php' ); // 要用相对路径指向 WordPress 根目录, 如果放在主题目录下, 则改为 ../../../wp-load.php
$d = get_bloginfo( 'wpurl' ). '/wp-content/avatar/default.png'; // 缺省头像的位置
$gravatarpath = "http://www.gravatar.com/avatar/";
$gravatarpath .= $hash;
$gravatarpath .= '?s=' . $s;
$gravatarpath .= '&d=' . urlencode( $d );
if ( !empty( $r ) )
    $gravatarpath .= '&r=' . $r;
$cachepath = ABSPATH . 'wp-content/avatar/' . $hash; // 缓存头像保存的位置
copy( $gravatarpath, $cachepath );
if ( filesize( $cachepath ) < 500) copy( $d, $cachepath );
{% endcodeblock %}

几点说明:

1. 第 8 行中 $key 为防止恶意提交的验证码, 请改成不容易破解的字符组合, 也不要告诉他人;
2. 如果文件不是放在根目录, 需要修改第 24 行的相对路径, 距离根目录多少层就打入多少 `../`, 主题目录为 `/wp-content/themes/theme`, 共有 3 层则为 `../../../wp-load.php; `
3. 没有为缓存的头像指定扩展名, 因为头像可能有多种格式. 实际上也不需要扩展名.

## 二、建立 Gravatar 头像存放的文件夹

根据刚才设定的缓存头像保存位置新建文件夹, 例如代码中将 Gravatar 头像存放在 wp-content/avatar 文件夹下, 因为我认为 wp-content 文件夹是用来存放用户生成内容的.

请将此文件夹的权限 (chmod) 设定为 755.

将无 Gravatar 头像时显示的缺省头像放在新建的文件夹中, 文件名要与上面代码中「缺省头像位置」一行一致.

## 三、写入 functions.php

打开主题目录下的 functions.php, 在 <?php 与 ?> 之间插入以下代码, 如果你已经用了 Willin Kan 的代码, 请替换:

{% codeblock lang:php %}
function my_avatar( $email, $size = '40', $default = '', $alt = false ) {
    $alt = ( false === $alt ) ? '' : esc_attr( $alt );
    $hash = md5( strtolower( $email ) );
    $wpurl = get_bloginfo( 'wpurl' );
    $out = $wpurl . '/wp-content/avatar/' . $hash; // 缓存头像的 URL
    $cachepath = ABSPATH . 'wp-content/avatar/' . $hash; // 缓存头像位于硬盘的位置
    $t = 1209600; // 设定 14 天, 单位: 秒
    $default = $wpurl . '/wp-content/avatar/default.png'; // 缺省头像的位置
    if ( !is_file( $cachepath ) || ( time() - filemtime( $cachepath ) ) > $t ){ // 当头像不存在或文件超过 14 天才更新
        $rating = get_option( 'avatar_rating' );
        $gravatarpath = "http://www.gravatar.com/avatar/$hash?s=$size&d=$default&r=$rating";
        $key = '123456'; // $key 为刚刚用于异步下载的 PHP 文件中设定的验证码
        $fp = fsockopen( $_SERVER['HTTP_HOST'], 80, &$errno, &$errstr, 5 ); // 建立 Socket 连接
        if ( $fp ) {
            $encoded = "hash={$hash}&s={$size}&r={$rating}&key=" . urlencode( $key );
            fputs( $fp, "POST ".get_bloginfo('wpurl')."/wp-content/themes/beam/gravatar.php HTTP/1.0\n" ); // 用于异步下载的 PHP 文件 URL
            fputs( $fp, "Host: ".$_SERVER['HTTP_HOST']."\n" );
            fputs( $fp, "Content-type: application/x-www-form-urlencoded\n" );
            fputs( $fp, "Content-length: " . strlen($encoded)."\n" );
            fputs( $fp, "Connection: close\n\n" );
            fputs( $fp, "$encoded\n" );
            fclose( $fp );
        }
        $out = esc_attr( $gravatarpath );
    }
    $avatar = "<img title='{$alt}' alt='{$alt}' src='{$out}' class='avatar avatar-{$size} photo' height='{$size}' width='{$size}' />";
    return apply_filters( 'my_avatar', $avatar, $email, $size, $default, $alt );
}
{% endcodeblock %}

几点说明:

1. 请将 $key 改为与第一步中指定的验证码一致, 否则程序不能工作;
2. 该函数调用方式与 WordPress 自带 get_avatar 已不相同;
3. get_avatar 语法:

{% codeblock lang:php %}
get_avatar( $id_or_email, $size = '<size>', $default = '<path_to_url>' , $alt = '<alt>' );
{% endcodeblock %}

my_avatar 语法:

{% codeblock lang:php %}
my_avatar( $comment->comment_author_email, $size = '<size>', $default = '<path_to_url>' , $alt = '<alt>' );
{% endcodeblock %}

您也可以将 WordPress 自带 get_avatar 复制并改造以保持原有功能;
4. 按照以上变更的调用方式, 将所有主题模板中出现的 get_avatar() 修改为 my_avatar(), 「大概是 functions.php, comments.php, sidebar.php, comments-ajax.php 会有头像的地方有 get_avatar() 函数. 」

## 四、测试

如果把文件放在与本文给出的位置不一样的地方, 要修改的地方会比较多, 最好在本地测试完成后上传到服务器.

P.s. 我知道代码花眼了点……想办法找个好用的代码高亮插件
