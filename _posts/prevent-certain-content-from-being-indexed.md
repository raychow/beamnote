---
title: "谈点 SEO: 阻止网页中某些内容被索引"
tags:
  - Google
  - SEO
  - 博客
id: 346
categories:
  - 技巧
date: 2011-02-24 08:00:17
---

自从建立博客以来, 我一直坚持做好内容, 少弄 SEO 的原则. 有些博客是「研究」SEO 的, 从我一年的观察看, 99% 的 SEO 博客内容都是千篇一律的: 刚开始的说些如何设置关键词, 然后聊聊内链、PR, 再说说优化谷歌、百度的注意事项, 最后没啥写的开始写诗歌散文的都有.

[![冰山](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2011/iceburg.jpg)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2011/iceburg.jpg)<!-- more -->

而今我正在写的, 也是关于 SEO 的内容. 以前我说过, 不喜欢写别人重复太多遍的内容, 这篇文章你也可以在网上搜搜, 找不着有人说这个话题.

在使用 [WP Microblogs](//beamnote.com/2011/wp-microblogs/) 插件时, 我发现了这个插件对搜索引擎带来的一点改变. 由于之前插件会直接将微博输出到网页, 导致微博内容被搜索引擎索引, 结果有一些流量是通过搜索与微博有关的内容带来的.

[![搜索 微博 关键字](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2011/microblog-search.jpg)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2011/microblog-search.jpg)

这张图被搜索的关键词, 是我在新浪转载的一条[微博](http://t.sina.com.cn/1691265967/5en0SpHilYL)中的内容. 访客通过这个关键词进来后很难找到他真正需要的内容, 多半是关闭窗口走人.

做博客我很讨厌几件事情, 其中之一是有人做任务式的留言, 这样的留言看起来毫无意义甚至让人厌恶, 看不爽的我会丢进垃圾桶; 二是不正常的流量, 无论是好文章没人看还是不相关的关键词来很多流量, 因为搜索引擎误解了我的博客.

看来是得做点什么阻止搜索引擎索引出现在博客网页中的微博. 我想到有些人做不公平友链的行为, 就是通过 JavaScript 方式输出链接, 访问者能够看到链接, 但是搜索引擎看不到, 起到了只输出流量不输出权重的作用.

搜索引擎真的看不到 JavaScript 里的内容么? 毕竟 JavaScript 是明文储存的, 索引某些文字也不无可能. 我找不到明确的答复, 我甚至在谷歌论坛发帖[询问](http://www.google.com/support/forum/p/webmasters/thread?tid=3a397e4333409ee6&amp;hl=zh-CN), 三天过去了也没有回音.

在这几天时间里, 我用自己的博客做了一个实验. 我的博客在谷歌有一点权重 (快照当天, 有一个关键词能展示[网站链接](https://www.google.com/support/webmasters/bin/answer.py?answer=47334&amp;hl=zh-CN), 尽管这个关键词是光线部落而不是新名字光线誌 :-| ). 开始实验前, 每天都有从搜索微博关键词带来的流量.

实验的方法, 是将微博改为 JavaScript 输出, 具体的步骤为:

1. 确定用于输出微博的元素, 例如:

    {% codeblock lang:xml %}
    <div id="microblog"></div>
    {% endcodeblock %}

2. 输出 JavaScript, 假定微博内容在 $content 变量中:

    {% codeblock lang:php %}
    $output = "<script type="text/javascript">n";
    $output .= "var microblog = "" . str_replace("r", '', str_replace("n", '', addslashes($content))). "";n";
    $output .= "document.getElementById('microblog').innerHTML = mc;n";
    $output .= "</script>";

    echo $output;
    {% endcodeblock %}

注意输出的时候要将输出内容中的引号与反斜杠前添加反斜杠 (使用 `addslashes()`), 还要去掉换行以免造成 JavaScript 语法错误.
现在您可以看看这个博客的源代码, 微博小工具就是用此方式输出的.

几天后, 我对实验效果进行了检查:

1. 查看流量分析, 已经没有从微博关键词带来的流量;
2. 查看网页快照, 纯文字版没有出现微博内容;
3. 在搜索引擎使用 `site` 语法搜索博客, 找不到与微博有关的内容.

可以确定, 使用 JavaScript 输出的方式确实可以阻止部分内容被索引. 但技术在发展, 也许不久的将来搜索引擎将逐步识别网页中的动态内容, 最后啥也逃不出 Google 的魔爪.
