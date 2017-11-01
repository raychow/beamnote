---
title: 向 Google Wave 迈进 (一)
tags:
  - Google
  - 主题
  - 博客
id: 242
categories:
  - 技巧
date: 2010-08-31 01:04:17
---

Google Wave 是 Google 又一项庞大的即将烂尾的工程, 也许是其理念太过超前, 当初的一码难求到现在 Google 宣布放弃, 我们鉴证了它的大起大落. Google Wave 也许将要倒下, 但它也许引领了未来的潮流; 今天我要借鉴的, 便是 Google Wave 的设计风.

[![向 Google Wave 迈进](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/forward-to-the-google-wave.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/forward-to-the-google-wave.png)<!-- more -->

这个主题从换域名上线到今天, 持续关注的朋友们会发现经常有很大的改变, 其实最初的样子是我自己用 PS 设计的, 但在寻找合适评论样式的时候我看中了 Google Wave. 虽然 Google 产品的风格多为极其简约, 不太适合模仿, 但 Wave 毕竟是一款创新理念的「平台」, 咱山寨一下还是不错的~

首先纵览一下 Google Wave 的全貌 (按惯例点击大图).

[![Google Wave](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/google-wave.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/google-wave.png)

这一章我们来看看右方的讨论区. Google Wave 中是可以随时发言即时刷新的, 也就是说你说完每句话别人都能立刻看见, 很像真正的会议——这功能当然是学不来的, 如前文所述, 我看中的是他的嵌套评论样式, 现在 (如果没有改变的话) 我正是模仿了它.

用到现在, 我对 Google Wave 评论样式的看法是:

* 分布清晰, 很容易看清是谁的回复;
* 外观简约, 不用考虑单双层或者不同层数之间的颜色等差异;
* 比例协调, 即使短回复也不会显得画面空荡荡;
* 较占空间, 尤其是评论下方要比其它样式多占用不少.

## 一、评论头像、昵称、时间、正文与回复按钮

如果评论回复采用 wp_list_comments 配合回调函数显示, 那么直接使用 CSS 就能实现, 不过我把多余的「×××说」给删了.

我的样式中, 除了头像为相对定位, 评论为默认定位方式, 其它均为绝对定位.

{% codeblock lang:css %}
#comments .avatar {
    position: relative;
    top: 3px;
}

#comments .fn {
    left: 50px;
    position: absolute;
    top: 15px;
}

#comments .fn a {
    color: #777;
    font-weight: bold;
}

#comments .comment-meta {
    left: 50px;
    position: absolute;
    top: 40px;
}

#comments .comment-meta a {
    color: #AAA;
}

#comments .reply {
    position: absolute;
    right: 5px;
    top: 15px;
}
{% endcodeblock %}

## 二、评论回复上方虚线

只要一句话就在合适的地方出现虚线啦, 不过位置还是得自己调整:

{% codeblock lang:css %}
#comments .comment-body {
    border-top: 1px dotted gray;
}
{% endcodeblock %}

## 三、评论回复边框

首先, 要在每个子评论 (ul.children) 的左方和下发设置一点内边距, 撑开评论实现错落有致的效果.

{% codeblock lang:css %}
#comments ul.children {
    padding: 0px 0px 40px 40px;
}
{% endcodeblock %}

接下来就是画灰色框线, 我们可以通过三种方法实现这样的效果.

### 1\. CSS

通过 CSS 阴影可以很容易的画出:

{% codeblock lang:css %}
ul.children {
    border-radius: 0 0 0 5px;
    -moz-border-radius: 0 0 0 5px;
    -webkit-border-radius: 0 0 0 5px;
    box-shadow: -3px 3px 4px #ccc;
    -moz-box-shadow: -3px 3px 4px #ccc;
    -webkit-box-shadow: -3px 3px 4px #ccc;
}
{% endcodeblock %}

大括号中前三行画出圆角, 后三行画出阴影, 针对 CSS3 标准与两种私有属性.

[![向 Google Wace 迈进 - CSS](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/gw-css.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/gw-css.png)

此方法:

* 评论框线不如 Google Wave 的自然;
* 无法支援 IE 浏览器……

2\. HTML + CSS + PHP

解决 HTML + CSS 先.

这是彻彻底底的山寨了……Google Wave 也是用的这方法: 放置了五个绝对定位的 DIV, 对应于评论框线的左上角、左方、左下角、下方、右下方, 结合图片达到效果.

HTML 是要在每个子评论 (ul.children) 中插入的, 与上方的 CSS 方法倒有些类似:

{% codeblock lang:html %}
<div class="shadow">
    <div class="fs bbfp bbnw"></div>
    <div class="yr bbfp bbw"></div>
    <div class="fs bbfp bbsw"></div>
    <div class="xr bbfp bbs"></div>
    <div class="fs bbfp bbse"></div>
</div>
{% endcodeblock %}

CSS 立刻奉上.

{% codeblock lang:css %}
.fs {
    background-image: url("images/shadow_n.png");
    background-repeat: no-repeat;
}
.yr {
    background-image: url("images/shadow_y.png");
    background-repeat: repeat-y;
}

.xr {
    background-image: url("images/shadow_x.png");
    background-repeat: repeat-x;
}

.bbfp {
    position: absolute;
}

.bbnw {
    background-position: 0 0;
    height: 40px;
    left: 0px;
    top: 0px;
    width: 6px;
}

.bbw {
    background-position: 0 0;
    bottom: 10px;
    left: 0px;
    top: 40px;
    width: 6px;
}

.bbsw {
    background-position: -6px 0;
    bottom: 0px;
    height: 10px;
    left: 0px;
    width: 9px;
}

.bbs {
    background-position: 0 0;
    bottom: 0;
    height: 10px;
    left: 9px;
    right: 60px;
}

.bbse {
    background-position: -6px -10px;
    bottom: 0;
    height: 10px;
    right: 0;
    width: 60px;
}
{% endcodeblock %}

图片在哪里呢? 你懂的.

下面解决大头, PHP. 很多主题使用 wp_list_comments 函数配合回调函数显示评论, 而回调函数尽管可以自定义评论样式, 但只能定义每一个列表项 (li), 不能控制 ul 或者 ol 的行为, 尽管您可以通过判断回调的 $depth 获得当前层数, 并将上面的代码插入到子评论中, 但由于没有参数反应当前列表项 (li) 是不是 ul 或者 ol 的第一项, 因此需要一点点判断.

判断的思路如下:

* 声明一个静态变量 $depthold, 存放上次的评论深度;
* 如果评论深度 ($depth) 为 1, 表示是最上层评论, 不需要插入评论框线;
* 如果评论深度 ($depth) 大于 1, 则与上次评论的深度相比:

    * 如果这次评论深度比上次的深, 表示是新一层评论, 需要插入 HTML;
    * 如果这次评论深度与上次一样, 表示是同一层评论, 不需要插入 HTML;
    * 如果这次评论深度比上次的浅, 表示该评论是从上次评论的某个父评论的同一层评论, 不需要插入 HTML.

* 将 $depthold = $depth, 准备下一次调用.

结合此思路, 给出代码:

{% codeblock lang:php %}
function beam_comment_list($comment, $args, $depth) {
    ……

    static $depthold;
    if ( $depth != 1 ) {
        if ( $depthold < $depth ) {
        ?>
        <div class="shadow">
        <div class="fs bbfp bbnw"></div>
        <div class="yr bbfp bbw"></div>
        <div class="fs bbfp bbsw"></div>
        <div class="xr bbfp bbs"></div>
        <div class="fs bbfp bbse"></div>
        </div>
        <?php
        }
    }
    $depthold = $depth;

    ……

}
{% endcodeblock %}

[![向 Google Wace 迈进 - PHP](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/gw-php.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/gw-php.png)

此方法:

* 无法支援 IE6;
* 但是比较精美啊！

3\. HTML + CSS + JavaScript

该方法 HTML 与 CSS 部分与第二点是相同的, 不同之处在于由 JS 代码插入 HTML.

{% codeblock lang:js %}
var allPageTags = new Array();
var allPageTags = document.getElementById("comments").getElementsByTagName("ul");
for ( i=0; i<allPageTags.length; i++ ) {
    var thisTag = allPageTags[i];
    if ( thisTag.className == "children" )
        thisTag.innerHTML = '<div class="shadow"><div class="fs bbfp bbnw"></div><div class="yr bbfp bbw"></div><div class="fs bbfp bbsw"></div><div class="xr bbfp bbs"></div><div class="fs bbfp bbse"></div></div>' + thisTag.innerHTML;
}
{% endcodeblock %}

运行效果与第二点相同.

此方法:

* 比第二点略省服务器资源;
* 浏览器禁用 JavaScript 时无法生效;
* 依然无法支援 IE6;
* 依然比较精美啊\!
