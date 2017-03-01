---
title: "向 Google Wave 迈进 (三) : 可记忆伸缩式小工具"
tags:
  - Google
  - 主题
  - 博客
id: 245
categories:
  - 技巧
date: 2010-09-06 00:29:36
---

向 Google Wave 迈进系列已经写到第三季了, 大概总是同一话题的关系, 评论人数节节攀低, 看来连续剧的缺点也不小……

不过各位放心, 这是最后一季了, 下次再弄点新东西. 主题敲敲打打到今天, 也积累了不少经验, 希望能有些拿得出手的 (话说, 现在的样子还是蛮有爱的吧\! 希望一天一变的外观没雷到您. 下面是这次用的背景图的原版……).

[![潮流背景](//img.beamnote.com/2010/forward-to-the-google-wave-chapter-3.jpg)](http://pic.onebizhi.com/bizhi/2008122817584060977801.jpg)<!-- more -->

可记忆伸缩式边栏其实与 Google Wave 无关, 但这是我模仿 Google Wave 时辛苦研究出来的花样, 于是并在一类写了. 平常见到的小工具动态效果多是边栏整体显示隐藏, 本文探讨的是通过 jQuery 对每个小工具的显示隐藏效果, 在这之前您可以先在本文右方动手试试, **带有标题栏的小工具可以单击标题隐藏并通过 Cookies 记忆状态**.

如[《向 Google Wave 迈进 (二) 》](http://beamnote.com/2010/forward-to-the-google-wave-chapter-2.html)所述, WordPress 提供的函数不能自定义小工具正文前后方的内容, 也就不能使用一个块级元素将正文包裹起来, 想要通过 jQuery 操作正文比较麻烦. 所幸的是我们可以指定小工具标题的样式, 所以我们可以通过 `:not()` 选择器反向选择所有正文内容, 进而实现显示隐藏的效果.

在开始操作之前, 请确保小工具的样式与[《向 Google Wave 迈进 (二) 》](http://beamnote.com/2010/forward-to-the-google-wave-chapter-2.html)中给出的一致.

## 一、jQuery 实现显示隐藏效果

{% codeblock lang:js %}
$('#sidebar .box-title').click( function() {
    var thisparent = $(this).parent();
    var thisroot = thisparent.children(':not(.box-title)');
    var idname = thisparent.attr('id');
    if ( thisroot.css('display') == 'none' ) {
        writeMenuCookie(idname, 0);
    } else {
        writeMenuCookie(idname, 1);
    }
    thisroot.slideToggle('normal');
});
{% endcodeblock %}

`:not(.box-title)` 选择了小工具的正文内容, 而 `writeMenuCookie` 是我定义的写入 Cookie 的函数.

## 二、writeMenuCookie 函数

从现在开始均是 JavaScript 代码, 不涉及 jQuery.

{% codeblock lang:js %}
function writeMenuCookie(idname, isadd) {
    if ( idname == '' ) return;
    var hidecookie = readCookie('menu_hide');
    if ( hidecookie.length == 0 ) hidecookie = ',';
    var idindex = hidecookie.indexOf(','+idname+',');
    if ( isadd ) {
        if ( idindex == -1 ) {
            hidecookie += idname + ','
        }
    } else {
        if ( idindex == 0 ) {
            hidecookie = hidecookie.slice(idindex+idname.length+1);
        } else if ( idindex > 0 ) {
            hidecookie = hidecookie.slice(0, idindex).concat(hidecookie.slice(idindex+idname.length+1));
        }
    }
    writeCookie('menu_hide', hidecookie, 8760);
}
{% endcodeblock %}

`writeCookie` 是我写入 Cookies 的函数.

对于 Cookies 处理我采用了比较取巧的一个办法 (我发现怎么又用了「取巧」两个字 ;-) ), 因为浏览器限制了存储 Cookies 的个数, 咱用的时候也要节省点, 对于一个 ID (即一个小工具) 占用一条 Cookie 的做法绝不考虑, 所以将 ID 存在一个字符串 (hidecookie) 里, 储存方式有点怪异, 以逗号开始, 以逗号分隔, 以逗号结束, 方便查找. 例如 `,ID1,ID2,ID3,`.

如果我要查询某个小工具 (例如 ID1) 是否要隐藏, 只要字符串寻找 Cookie 中 `,ID1,` 是否存在即可, 不用数组更高效~

## 三、Cookies 操作函数

这函数网上一大堆, 为了方便就直接贴出来了.

{% codeblock lang:js %}
function readCookie(name) {
    var arr = document.cookie.match(new RegExp('(^| )'+name+'=([^;]*)(;|$)'));
    if ( arr != null )
        return unescape(arr[2]);
    else
        return '';
}
function writeCookie(name, value, hours) {
    var str = name + '=' + escape(value);
    if (hours != null) {
        expire = new Date((new Date()).getTime() + hours * 3600000);
        str += '; expires=' + expire.toGMTString();
    }
    str += '; path=/';
    document.cookie = str;
}
{% endcodeblock %}

从网上搜刮的, 不解释.

## 四、让记忆重现——服务器端识别并输出隐藏小工具的 CSS

既然进展到服务器了, _COOKIE 这个数组必不可少, 它包含了所有的 Cookies.

把此处代码写到 functions.php 里即可.

{% codeblock lang:js %}
function beam_sidebar_style() {
    $hidecookie = $_COOKIE['menu_hide'];
    if ( strlen($hidecookie) < 3 ) return;
    $alwayshow = array('text-6', 'widget_beam_phptext-4');
    $out = "";
    $idindex = 1;
    do {
        $newidindex = strpos($hidecookie, ',', $idindex);
        if ( $newidindex ) {
            $idname = substr($hidecookie, $idindex, $newidindex-$idindex);
            if ( !in_array( $idname, $alwayshow ) )
                $out .= "#$idname > *, ";
            $idindex = $newidindex + 1;
        }
    } while ($newidindex);
    $out = substr($out, 0, strlen($out) -2 );
    $out = "<style>\n$out { display: none; }\n</style>\n";
    return $out;
}
{% endcodeblock %}

CSS 的选择器是 `#ID > *`, 只选择第一代子元素, 不然把子代全部隐藏后就显示不出来啦.

其中有一个 $alwayshow 数组, 存放的是你不想隐藏的小工具 (例如广告 8-) ).

最后, 在 `header.php` 的头部 (`<head>` 与 `</head>` 之间) 插入这一句:

{% codeblock lang:php %}
<?php print beam_sidebar_style(); ?>
{% endcodeblock %}

至此, 如果没出错的话, 您就可以使用能够伸缩, 并且刷新后依然保持原样的小工具了.
