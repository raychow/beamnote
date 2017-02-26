---
title: Chrome 新功能速递
tags:
  - Chrome
  - Google
  - 测试
  - 网络
id: 192
categories:
  - 关注
date: 2010-06-19 20:23:27
---

更新频繁的 Chrome Dev 在 6.0.437.3 又有大动作, 加入了几个让人期待已久的功能, 并且进一步简化界面, 让我们一探究竟吧~

先说说界面的变化 (点击大图, 本站惯例) :

[![Chrome 6.0.437.3 dev](http://img.beamnote.com/2010/chrome-whats-new.png)](http://img.beamnote.com/2010/chrome-whats-new.png)<!-- more -->

## 界面

原来三角形的转到按钮被去除, 刷新按钮集成了停止功能, 放在地址栏左边.

这个设计我不喜欢, 首先单击就可以转到网址的功能没有了, 输入一个网址后只能按回车转向; 另外, 将这个按钮放在长长的地址栏左边, 由于鼠标需要拖动在右边的滚动条, 一左一右来回很不方便.

## 新功能

* 内置 PDF 阅读插件
* 扩展程序同步
* 自动填写同步 (貌似 Windows 下没有, Mac OS 下也不知道是什么时候加入的)
* CSS 3D 变换

### 内置 PDF 阅读插件

这个功能对于没有预装 PDF 阅读器的系统很有用, 我再也不必为了偶尔看一下的 PDF 文档专门下载一个阅读器了.

在地址栏输入 chrome://plugins/, 打开插件管理器, 可以启用 Chrome PDF Viewer.

[![书签管理器](http://img.beamnote.com/2010/2010-06-19_23-24-05.png)](http://img.beamnote.com/2010/2010-06-19_23-24-05.png)

直接输入 PDF 文件就可以打开:

[![Chrome PDF](http://img.beamnote.com/2010/2010-06-19_23-24-29.png)](http://img.beamnote.com/2010/2010-06-19_23-24-29.png)

与系统自带阅读器相比, 字体渲染的难看一点.

[![Chrome PDF](http://img.beamnote.com/2010/2010-06-19_23-27-42.png)](http://img.beamnote.com/2010/2010-06-19_23-27-42.png)

### 扩展程序同步

[![Chrome 新功能](http://img.beamnote.com/2010/2010-06-19_19-13-47.png)](http://img.beamnote.com/2010/2010-06-19_19-13-47.png)

这可是千呼万唤始出来的亮点啊, 6.0.437.2 有乌龙 Bug 紧急回炉, 6.0.437.3 版应该没问题吧.

要开启此功能, 请加入以下启动参数:
> --enable-sync-extensions --sync-url=https://clients4.google.com/chrome-sync/dev
据说扩展程序同步只是从官方扩展程序中心下载, 所以不能同步扩展程序的设定 :-( .

### 自动填写同步

我还暂时用不到这功能, 具体自测吧.

要开启此功能, 请加入以下启动参数:
> --enable-sync-autofill

### CSS 3D 加速

这个可就噱头了, 它允许使用 CSS 在三维空间定位元素 (不过貌似是 WebKit 专属代码).

在 [http://webkit.org/blog/386/3d-transforms/](http://webkit.org/blog/386/3d-transforms/) 中有一张数字环绕成的图片, 在支持此功能的浏览器中, 鼠标移到目标, 图片会自动 3D 翻转——不需要任何 JavaScript 支持.

在 [http://webkit.org/blog-files/3d-transforms/morphing-cubes.html ](http://webkit.org/blog-files/3d-transforms/morphing-cubes.html)中有一个数字魔方, 在支持此功能的浏览器中, 魔方是正常显示并旋转的. 单击 Toggle Shape, 魔方会转变为转动的十二面体.

要开启此功能, 请加入以下启动参数:
> --enable-accelerated-compositing
* <del datetime="2010-06-19T16:05:14+00:00">貌似我没法开启</del>, 目前只能在 Windows 中开启. 以下是 Safari 中演示图片:

[![3D Transforms](http://img.beamnote.com/2010/2010-06-19_08-02-44.png)](http://img.beamnote.com/2010/2010-06-19_08-02-44.png)

[![3D Transforms](http://img.beamnote.com/2010/2010-06-19_08-03-05.png)](http://img.beamnote.com/2010/2010-06-19_08-03-05.png)

[![3D Transforms](http://img.beamnote.com/2010/2010-06-19_08-03-21.png)](http://img.beamnote.com/2010/2010-06-19_08-03-21.png)

[![3D Transforms](http://img.beamnote.com/2010/2010-06-19_08-03-35.png)](http://img.beamnote.com/2010/2010-06-19_08-03-35.png)

Chrome 中美图欣赏:

[![3D Transforms](http://img.beamnote.com/2010/2010-06-19_20-10-08.png)](http://img.beamnote.com/2010/2010-06-19_20-10-08.png)

最后附上几个我还没有介绍的 Chrome 扩展:

iPageRank: 显示当页 PR, 移上图标还有惊喜;

Readability Redux: 进入读报纸模式, 保护视力 ;

Ultimate Chrome Flag: 显示服务器所在地, 单击图标也有惊喜;

迅雷、快车、旋风专用链破解工具: 不言而喻, 直接把该死的专用链置换成普通链接;

中国天气预报与万年历: 很方便的查看天气预报与温度, 单击图标还有惊喜.
