---
title: 换域名换站名换主题
id: 238
categories:
  - 唠叨
date: 2010-08-20 23:57:29
tags:
---

呼呼, 弄了一个多星期终于把一切都搞定了, 不容易啊, 所以在这里宣告一下.

[![域名、站名、主题](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/domain-name-theme.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/domain-name-theme.png)<!-- more -->

## 一、换域名

我的博客域名本来是 http://raychow.info, .info 属于轻微非主流性质的域名后缀, 再加上 GoDaddy 弄到 .info 只卖 $0.89 的贱价, 似乎好不值钱.

建站买域名的时候翻了好久 .com 米, 中意的都被注册了, 所幸有个米 rayblog.com 不久后将要到期, 内容也很久没更新了, 看来有戏\! 苦等了六个多月, 快要挂掉的米居然被续费了, 好久都不写东西了还续费做什么? ！

让我再次开动大脑想吧……查了 n 多次字典最终找到一个长度不算太长又有含义的 http://beamnote.com, 撒花庆祝 :-D .

域名直白到不用解释, Beam 光线, Note 誌, 哈！

这次换域名不换主机简单到难以想象, 总之比网路上搜到的都要容易, 虽然已经是月经贴了, 还是顺便提一下:

1. 把解析做好, 域名解析到主机上, 然后在主机后台面板设定停放域名, 停放到原域名一个目录;
2. 等 DNS 反应过来你会发现两个域名都能访问了;
3. 做 301 重定向, 别忘了在谷歌网站管理员工具里通知谷歌, 重定向直接写在网站根目录的 .htaccess 文件尽可能的前方:
{% codeblock lang:bash %}
RewriteCond %{HTTP_HOST} ^raychow.info [NC]
RewriteRule (.*) http://beamnote.com/$1 [R=301,L]
RewriteCond %{HTTP_HOST} ^www.raychow.info [NC]
RewriteRule (.*) http://beamnote.com/$1 [R=301,L]
{% endcodeblock %}

4. 进入 WordPress 后台把安装地址和博客地址改过来, 再进入数据库把所有出现原域名的地方替换成新域名
{% codeblock lang:sql %}
UPDATE wp_posts SET post_content = REPLACE (
    post_content,
    'http://exampleoldsiteurl.com',
    'http://examplenewsiteurl.com');
UPDATE wp_posts SET guid = REPLACE (
    guid,
    'http://exampleoldsiteurl.com',
    'http://examplenewsiteurl.com');
{% endcodeblock %}
这样就结束了, 连文件都不动一个. 但是还有两个疑问:

1. .htaccess 文件必须写两遍 RewriteRule, 如果合在一起 (就是删掉 RewriteRule (.*) http://beamnote.com/$1 [R=301,L] 一行), 就只能重定向最靠近 RewriteRule 的一项了, 我差点中招……这文件语法就是这样么?
2. 数据库操作中有替换 GUID 字段一步, GUID 应该是全球唯一标识符, 各种程序可以根据时间、网卡物理地址等信息生成一个不重复的字符串, 用在 WordPress 上应该就是储存固定链接的字段了, 其实我正在码字的时候这一字段并未替换, 但文章依然能正常通过固定链接定向, 这个字段究竟起何作用?

## 二、换站名

没什么可解释的, 觉得光线部落四个字长了点, 改成三个字了, 正好和域名贴切.

P.s. 如果之前申请到 rayblog.com, 现在就可能改名日不落啦 :-| ！

## 三、换主题

这主题的名字就是 Beam, 正是我辛辛苦苦折腾了一个星期的成果. 但做主题和换域名没有任何联系, 只是凑巧同时发生了.

Beam 属于不多见的左对齐简洁型主题, 用了一丁点儿 jQuery 效果.

我承认这主题还没有完工, 因为我做它做到实在很烦, 谁知道 CSS 那么麻烦, 尤其是对 IE6 的兼容\! 今天几乎一天时间都用在调教 IE6 上, 尽管针对 IE6 的 CSS 也就那么一点儿. 现在主题在 IE6 下显示比较正常.

主题就算正式上线了, 不过会和 Chrome 与 QQ 一样, 长期处于 Beta 状态 (现在版本号为 0.5).

大家有问题尽管提, 我先去休息一下了……

现在还有已知的一点问题:

* LightBox 用到的图片挂了 (很快修复);
* IE6 下新评论的头像不显示 (IE6 大 Bug, 真的懒的修复了);
* 评论样式什么的几乎没写;
* 行距什么的, 几乎是默认值, 都帮忙看看吧.

## 晕, 最重要的事情忘记了

那就是各位友链们看到了麻烦改一下链接到 http://beamnote.com.

订阅链接暂时保持不变, http://feed.raychow.info, 过两天再调整.

即使你不记得新域名, 原来的 http://raychow.info 也为你保留到 2012 之后喔\!
