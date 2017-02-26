---
title: 向 Google Wave 迈进 (二)
tags:
  - Google
  - 主题
  - 博客
id: 243
categories:
  - 技巧
date: 2010-09-03 15:15:46
---

写博客最烦没话题写, 所以抓住一个亮点就要多加挖掘. 为了实现话题的可持续发展, 把一个大的内容拆成多集连续剧来写, 推荐大家都来试试这招.

上集中我们讨论了山寨 Google Wave 的评论样式, 这一集的剧情是, 实现 Google Wave 的窗口.

我的主题目前套用 Wave Style 的地方有左边的分类导航与右边的小工具. 继续拿上次的那张图来:

[![Google Wave](http://img.beamnote.com/2010/google-wave.png)](http://img.beamnote.com/2010/google-wave.png)

## <!-- more -->一、标题栏的色彩

窗体的标题栏为蓝色由浅向深色的渐变, Google Wave 采用 CSS 3 的线性渐变实现 (没测试, 不知道这样写能不能正常工作) :

{% codeblock lang:css %}
background-color: #5590d2;
background-image: linear-gradient(#61A7F2, #5590D2);
background-image: -moz-linear-gradient(#61A7F2, #5590D2);
background-image: -webkit-gradient(#61A7F2, #5590D2);
{% endcodeblock %}

不过为了兼容 IE, 还是用图片实现吧, 否则在 IE 下就只有单色咯

## 二、窗体结构

说到这个窗体的结构, 还真的苦恼了我一阵, 主要在于 WordPress 对于小工具的输出控制.

本来为了方便实现窗体展开伸缩的 jQuery 特效, 我将标题与内容放在两个 DIV 中, 套用在小工具时出现了难题. WordPress 只允许小工具初始化时通过 `register_sidebar` 自定义以下内容:

* before_widget: 每个小工具之前的代码;
* after_widget: 每个小工具之后的代码;
* before_title: 标题之前的代码;
* after_title: 标题之后的代码;
* 其它描述性内容.

而没有 before_content, after_content 之类自定义内容前后代码的参数. 取巧的将 <div class="content"> 放在 after_title 的结尾, </div> 放在 after_widget 的开头, 却不幸的发现有些小工具 (例如文本小工具) 是没有标题的, 此时 before_title 与 after_title 均不会输出, 导致多出来一个 </div> 提前结束了 DIV 块导致网页严重变形.

而后经研究决定 ;-), 只保留标题前后的 DIV, 从 jQuery 上想办法.

那么, `register_sidebar` 函数最终传递的参数是这样:

{% codeblock lang:php %}
$p = array(
    'before_widget'  =>   '<li id="%1$s" class="box-border radius widget %2$s">',
    'after_widget'   =>   '</li>',
    'before_title'   =>   '<div class="block box-title radius-top"><h3>',
    'after_title'    =>   '</h3></div>'
);
register_sidebar( $p );
{% endcodeblock %}

## 三、样式

{% codeblock lang:css %}
/* 圆角 */
.radius {
    border-radius: 5px;
    -moz-border-radius: 5px;
    -webkit-border-radius: 5px;
    -khtml-border-radius: 5px;
}
/* 标题栏上半圆角 */
.radius-top {
    border-radius: 5px 5px 0 0;
    -moz-border-radius: 5px 5px 0 0;
    -webkit-border-radius: 5px 5px 0 0;
    -khtml-border-radius: 5px 5px 0 0;
}

/* 让内容显示为块级元素, 否则无法收起 */
.block {
    display: block!important;
}

/* 整个窗体的底色、阴影 */
.box-border {
    box-shadow: 3px 3px 6px #AAAAAA;
    -moz-box-shadow: 3px 3px 6px #AAAAAA;
    -webkit-box-shadow: 3px 3px 6px #AAAAAA;
    background-color: #fff;
    margin-bottom: 15px;
    padding: 4px 8px;
}

/* 窗体标题栏 */
.box-title {
    background:url("images/box-title.png") repeat-x;
    color: #fff;
    cursor: pointer;
    font-size: 1em;
    height: 22px;
    margin: -4px -8px 0;
    padding: 3px 8px 4px;
}

.box-title h3 {
    font-size: 1em;
    margin: 0;
}

.box-title a {
    color: #fff;
}
{% endcodeblock %}

此样式是我现在使用的, 可能各间距不太适合您的主题, 稍加修改即可.
