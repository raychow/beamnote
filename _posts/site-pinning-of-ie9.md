---
title: Site Pinning of IE9
tags:
  - IE
  - 微软
  - 网络
id: 262
categories:
  - 技巧
date: 2010-10-23 15:18:14
---

IE9 测试版已经出来一段时间了, 它完善了对 HTML5、CSS3 的支持, 简化 (丑化? ) 了界面, 增强了安全措施, 不过在这个百花齐放的时代, 这些也不算什么亮点. 如果要说什么最独特的地方, 估计也就是今天提到的 [Site Pinning](http://windows.microsoft.com/zh-CN/internet-explorer/help/ie-9/whats-new-in-internet-explorer-9#section_2) (固定网站) 了, 它能够将一个网站如同程序固定在 Windows Vista / 7 的任务栏中, 对于支持此功能的网站, 还能定义图标、导航按钮颜色、Jump List、Thumbnail Toolbar 等内容.

[![Site Pinning of IE9](http://img.beamnote.com/2010/site-pinning-of-ie9.png)](http://img.beamnote.com/2010/site-pinning-of-ie9.png)<!-- more -->

微软提供了一系列方法使得网站支持 Site Pinning, 可以参见以下两个 Demo.

1. 通过 Meta 定义程序名称、描述、导航按钮颜色、Jump List 工作列表、起始页, 请[按此](/demo/ie9/demo.html);
2. 通过 JavaScript 动态定义覆盖图标、Jump List 与 Thumbnail Toolbar, 请[按此](/demo/ie9).

这两个 Demo 都需要使用 IE9 浏览, 并将标签拖动到任务栏固定后才能观察效果.

## 一、通过 Meta 定义

这个还是蛮简单的, 一看就明白.

{% codeblock lang:html %}
<meta name="application-name" content="IE9 Test" />
<meta name="msapplication-tooltip" content="Ray's IE9 Test" />
<meta name="msapplication-window" content="width=1024;height=768" />
<meta name="msapplication-task" content="name=主页;action-uri=/;icon-uri=/favicon.ico"/>
<meta name="msapplication-task" content="name=留言;action-uri=/guestbook;icon-uri=/favicon.ico" />
<meta name="msapplication-task" content="name=关于;action-uri=about;icon-uri=/favicon.ico" />
<meta name="msapplication-navbutton-color" content="#0000ff" />
<meta name="msapplication-starturl" content="/" />
{% endcodeblock %}

## 二、通过 JavaScript 定义

主要是用到了 IE9 提供的几个 API, 所以肯定要做错误处理, 不然换成其它浏览器就死翘翘了 (喂, 这又是 IE Only 耶).

1. `window.external.msIsSiteMode()`
    返回当前 Internet Explorer 窗口是否作为「固定网站」打开.
    使用这个方法区别正常浏览模式与「固定网站」模式.
2. `window.external.msAddSiteMode();`
    将当前网站添加到开始菜单, 并将网站作为「固定网站」打开, 但没有固定到任务栏.
3. `window.external.msSiteModeSetIconOverlay(bstrIconUrl [, bstrDescription]);`
    添加「覆盖图标」.
    bstrIconUrl: 图标 URL;
    bstrDescription: 提供图标的描述.
4. `window.external.msSiteModeClearIconOverlay();`
    清除「覆盖图标」.
5. `window.external.msSiteModeCreateJumpList(bstrHeader);`
    创建一个新的 Jump List, 并为其指定名称.
    只能够创建一个 Jump List.
6. `window.external.msSiteModeClearJumpList();`
    清除 Jump List.
7. `window.external.msSiteModeAddJumpListItem(bstrName, bstrActionUri, bstrIconUri);`
    增加一个 Jump List 项目, 最多可以存在 20 个 Jump List 项目.
    bstrName: 显示的名称;
    bstrActionUri: 单击时转向的绝对或相对 URL;
    bstrIconUri: 显示的图标绝对或相对 URL.
8. `window.external.msSiteModeShowJumpList();`
    更新 Jump List.
    对 Jump List 作出修改后, 使用这个方法更新显示.

---

以下是实现 Thumbnail Toolbar 的方法.

1. 为 msthumbnailclick 事件设置监听.

    {% codeblock lang:js %}
    document.addEventListener('msthumbnailclick', onButtonClicked, false);
    {% endcodeblock %}

2. 添加按钮. 这个函数将返回按钮 ID.

    {% codeblock lang:js %}
    var btnPlay = window.external.msSiteModeAddThumbBarButton(iconUri, toolTip);
    {% endcodeblock %}

3. 显示 Thumbnail Toolbar.

    {% codeblock lang:js %}
    window.external.msSiteModeShowThumbBar();
    {% endcodeblock %}

4. 对 Thumbnail Toolbar 的单击事件作出响应.

    {% codeblock lang:js %}
    function onButtonClicked(e) {
        switch (e.buttonID) {
            case btnPlay:
            play();
            break;
        }
    }
    {% endcodeblock %}
