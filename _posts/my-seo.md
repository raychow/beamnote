---
title: 我优化搜索引擎的方法
tags:
  - WordPress
  - 博客
id: 71
categories:
  - 技巧
date: 2010-02-10 15:14:10
---

昨天我使用站长工具检查我的站点, 发现 Meta 信息中的 Keywords 全部为空, 因为安装的 Mystique 主题认为不需要针对 Keywords 优化. 我搜索了一下, 谷歌收录时的确不考虑 Keywords, 但在中国更多的人使用百度进行搜索, 而百度是什么态度我不清楚, 还是加上 Keywords 描述比较好. 很多人推荐了 All in One SEO Pack, 我安装了, 发现它最主要的功能就是对每一页的标题、描述、关键词进行优化. 有人说对分类页、存档页、标签页设置了 noindex 之后出现整个网站被百度K掉只剩首页的现象, 尽管不太相信, 我依然觉得没必要通过安装插件优化搜索引擎, 决定修改主题的源代码人工实现. 以下内容由我搜集材料并根据本站点实际情况写出: <!-- more -->

## 一、标题

在头部文件 header.php 中相应位置修改:

{% codeblock lang:php %}
<title><?php if (is_home()) { echo bloginfo('name'); } else { echo trim(wp_title('',false)); ?> - <?php echo bloginfo('name'); } ?></title>
{% endcodeblock %}

首页标题为博客名称; 其他页面的标题为「页面标题 - 博客名称」.

## 二、描述与关键词

在头部文件 header.php 中相应位置修改:

{% codeblock lang:php %}
<?php
if ( is_home() ) {
    $description = "「光线部落」是一个专注互联网应用与行业动态的独立博客. ";
    $keywords = "光线部落, 博客, 互联网, 评论, 计算机, 软件";
} elseif ( is_single() ) {
    $description = $post->post_excerpt;
    $keywords = "";
    $tags = wp_get_post_tags( $post->ID );
    foreach ( $tags as $tag ) {
        $keywords = $keywords . $tag->name . ", ";
    }
    $keywords = substr( $keywords, 0, -2 );
}
?>
<meta name="description" content="<?php echo $description; ?>" />
<meta name="keywords" content="<?php echo $keywords; ?>" />
{% endcodeblock %}

首页的描述与关键字在代码中直接修改;

文章页面的描述与关键字对应文章本身的「摘要」与「文章标签」, 在编辑文章时手动设定;

其他页面 (如分类、存档、标签页面) 均未设置描述与关键词, 因为不需要被搜索引擎收录.

如果不想手工指定文章的描述, 可以使用下列代码, 自动截取开头 220 个字符作为描述, 但关键字仍需手动指定:

{% codeblock lang:php %}
<?php
if ( is_home() ) {
    $description = "「光线部落」是一个专注互联网应用与行业动态的独立博客. ";
    $keywords = "光线部落, 博客, 互联网, 评论, 计算机, 软件";
} elseif ( is_single() || is_page() ) {
    if ( $post->post_excerpt ) {
        $description = $post->post_excerpt;
    } else {
        $description = mb_strimwidth( strip_tags( apply_filters( 'the_content', $post->post_content ) ), 0, 220 );
    }
    $keywords = "";
    $tags = wp_get_post_tags( $post->ID );
    foreach ( $tags as $tag ) {
        $keywords = $keywords . $tag->name . ", ";
    }
    $keywords = substr( $keywords, 0, -2 );
}
?>
<meta name="description" content="<?php echo $description; ?>" />
<meta name="keywords" content="<?php echo $keywords; ?>" />
{% endcodeblock %}


## 三、控制机器人抓取页面

分类、存档、标签页面会被搜索引擎认为是重复内容, 当然不能抓取.

在头部文件 header.php 中相应位置修改:

{% codeblock lang:php %}
<?php if ( is_single() || is_page() || is_home() ) { ?>
    <meta name="robots" content="index, follow" />
<?php } else { ?>
    <meta name="robots" content="noindex, follow" />
<?php }; ?>
{% endcodeblock %}

如果担心被百度全部删除的话, 使用下列代码只控制谷歌抓取 (其实我觉得不太可能会被 K 掉) :

{% codeblock lang:php %}
<?php if ( is_single() || is_page() || is_home() ) { ?>
    <meta name="robots" content="index, follow" />
<?php } else { ?>
    <meta name="googlebot" content="noindex, follow" />
<?php }; ?>
{% endcodeblock %}
