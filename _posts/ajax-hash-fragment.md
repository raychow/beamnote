---
title: 让 AJAX 程序通过 URL 保持追踪
tags:
  - AJAX
  - 网络
id: 274
categories:
  - 技巧
date: 2010-12-13 14:05:12
---

AJAX 现在很流行, 按 Google 的话说, 越来越多的网页从静态页面转变为基于 AJAX 的应用. 虽然带来了方便, 同时一些问题也随之产生. 今次要讨论的就是 AJAX 程序通过地址栏保持追踪的问题.

「通过 URL 保持追踪」是我个人的描述, 详细解释是通过修改 URL 达到控制 AJAX 程序 (或者单纯的 JavaScript 程序) 的目的. 很多网站使用了 AJAX 技术, 在页面内翻页数次后看到感兴趣的内容, 如果通过此 URL 分享, 其他人打开的仍然是第一页, 这就给使用带来一定麻烦; 同样的, 前进后退功能被破坏了, 单击后退按钮可能没有反应, 也可能跳转到前一个页面而非此页面中最近浏览的内容.

[![hash](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/ajax-hash-fragment.jpg)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/ajax-hash-fragment.jpg)<!-- more -->

维基百科中[指出](http://zh.wikipedia.org/zh-cn/Ajax), 「使用动态页面更新使得用户难于将某个特定的状态保存到收藏夹中」, 即是刚刚描述的情况, 解决方法也被一笔带过: 「使用 URL 片断标识符 (通常被称为锚点, 即 URL 中 # 后面的部分) 来保持追踪, 允许用户回到指定的某个应用程序状态」, 「这些解决方案也同时解决了许多关于不支持后退按钮的争论」.

使用了这一技术的典型网页如 Google Instant Search、Gmail、Twitter 新界面等.

要通过锚点保持追踪, 需要关于锚点改变的事件. 遗憾的是, 虽然存在一个 `[hashchange](https://developer.mozilla.org/en/dom/window.onhashchange)` 事件, 但他是一个 HTML5 的规范, 只在 Internet Explorer 8, Firefox 3.6, Chrome 5 以上版本支持. 要支持低版本, 需要自己创建 hashchange 事件.

JavaScript 不可能像 Windows API 那样进行 hook, 只有创建一个定时器每隔一段时间检查锚点 (hash) 是否改变, 支付宝前端开发组的博客对此作出了[研究](http://ued.alipay.com/wd/2009/12/17/hash-fragment/):

{% codeblock lang:js %}
hashWatcher = function() {
    var timer, last;
    return {
        register: function(fn, thisObj) {
            if(typeof fn !== 'function') return;
            timer = window.setInterval(function() {
                if(location.hash !== last) {
                    last = location.hash;
                    fn.call(thisObj || window, last);
                }
            }, 100);
        },
        stop: function() {
            timer &amp;&amp; window.clearInterval(timer);
        },
        set: function(newHash) {
            last = newHash;
            location.hash = newHash;
        }
    };
}();
{% endcodeblock %}

将事件处理函数置于 `hashWatcher.register()` 回调中, 如

{% codeblock lang:js %}
hashWatcher.register(function(hash) {
    document.body.bgColor = hash;
});
{% endcodeblock %}

页面背景颜色将随 `hash` 改变, [请看这个 Demo](/demo/js-anchor/index2.html) (点击链接或改变 URL 中的 hash 都可以改变颜色).

使用 `hashWatcher.set()` 方法更新 `hash` 可以避免触发事件.

这里存在 IE 6 / 7  以及 IE 8 (兼容模式) 的兼容性问题, 具体表现为: 不能使用浏览器的前进后退按钮保持追踪, 因为这些版本浏览器并不为锚点的改变创建历史——他们认为这是同一个页面.

要解决这个问题, 可以创建一个隐藏的 `iframe`, 通过改变 `iframe.location.hash` 控制 IE 的历史记录, 有人将这个想法完善并做成了完整的 jQuery 插件, 请参见[《jQuery hashchange event》](http://benalman.com/projects/jquery-hashchange-plugin/). 这款插件还针对各浏览器采取了相应的处理方法, 原生支持 `hashchange` 事件的, 将事件处理函数绑定到事件上; 不支持 `hashchange` 事件的, 采用定时器检测法.

[查看 jQuery hashchange event 的 Demo](/demo/js-anchor/)

这个 Demo 在低版本 IE 浏览器中也可以使用前进后退按钮改变颜色.

AJAX 还导致了搜索引擎收录问题, 因为搜索引擎不能执行 JavaScript 脚本, 动态生成的内容无法被收录. 下一篇我就对这一问题, 结合 Google Code 中[《Making AJAX Applications Crawlable》](http://code.google.com/intl/en-US/web/ajaxcrawling/docs/learn-more.html)与大家一起探讨.
