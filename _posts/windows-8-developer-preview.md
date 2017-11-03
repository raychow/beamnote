---
title: Windows 8 Developer Preview 尝鲜
tags:
  - Windows
  - 系统
id: 397
categories:
  - 分享
date: 2011-09-16 13:45:41
---

微软在 14 号开放了 Windows 开发者版本的下载, 这是版本虽然不具备所有 Windows 8 新特性, 但新功能还是有一些噱头的.

## 一、安装

原本我打算将 Windows 8 安装在虚拟机中, 可惜这个版本的 Windows 居然与各种虚拟机不兼容: VMWare 7 及更早版本铁定没戏, 直接提示「HAL_INITIALIZATION_FAILED」错误, 换到 VMWare 8 Beta 之后倒是可以安装了, 但全程 CPU 占用 100%, 进入系统时漫漫无期的等待让我果断放弃 (今天 VMWare 8 正式发布了, 有兴趣的同学可以试试); 不少网友说 Oracle 的 VirtualBox 可以安装, 可能因为我的电脑 CPU 不支持硬件虚拟化, 进入安装程序时也遭到失败.

[![HAL_INITIALIZATION_FAILED](//img.beamnote.com/2011/HAL_INITIALIZATION_FAILED.jpg)](//img.beamnote.com/2011/HAL_INITIALIZATION_FAILED.jpg)<!-- more -->

后来我在 cnBeta 上找到一篇文章《[Windows 8 Developer Preview 完美与 Windows 7 共存法安装指南](http://cnbeta.com/articles/155176.htm)》, 将系统安装在 VHD 中, 总算搞定.

注: pcBeta 上的《[完整 Windows 7 下 VHD 安装 Windows 8 图文教程](http://bbs.pcbeta.com/viewthread-916774-1-1.html)》适用于没有额外可引导设备的情况, 但需要手动修改引导.

## 二、启动

Windows 8 Developer Preview 的启动画面换成了半圆环阵列的点, 与苹果的菊花 (风火轮) 不同, 这些点是不对称的, 比较卡的时候他们看起来运动的毫无规律.

[![Windows 8 Loading](//img.beamnote.com/2011/windows8-loading.png)](//img.beamnote.com/2011/windows8-loading.png)

 (实际上下方的点正在向圆的轨迹运动, 图片不是实际的启动画面, 而是 Windows Build 的启动画面)

系统中多处运用了这种圆环式的, 以及另一种横向散列点的载入动画.

## 三、初识

如果设置了密码 (例如绑定了 Windows Live ID), 会显示一个全屏的锁定界面. 由于无法截图, 在网上找了一张图片: 纯粹的 Metro 设计——一张风景图, 左下角是大大的时间与其它图标.

[![Windows 8 Loading](//img.beamnote.com/2011/windows8-lockscreen.jpg)](//img.beamnote.com/2011/windows8-lockscreen.jpg)

按住鼠标向上拖动, 输入密码后进入 Metro 界面.

[![Windows 8 Metro](//img.beamnote.com/2011/windows8-metro.jpg)](//img.beamnote.com/2011/windows8-metro.jpg)

别看首页的图标五彩缤纷, 你自己安装进去的程序都是很丑的……如同 Internet Explorer 与 Windows Explorer 那样.

前面几个是线上应用: 天气、股票、咨询、脸书、推特, 一个不能用的 Windows Store, 和不知为何无法登入的 Windows Build. 后面的是制作简陋适合触摸屏使用的游戏及本地应用. 相信在正式版推出时这里会变得很有趣.

其中「Desktop」点进去就切换回了桌面:

[![Windows 8 Desktop](//img.beamnote.com/2011/windows8-desktop.jpg)](//img.beamnote.com/2011/windows8-desktop.jpg)

开始按钮单击之后不再弹出开始菜单, 而是回到 Metro 界面 (可以改回开始菜单). 移动到开始按钮, 会出现快捷命令和时间.

## 四、碎语

个人观点, 如有错误, 纯属认识肤浅.

这个系统给我的感觉是: Windows 7 SP2 + Metro Shell. 由于大多数程序并未开发 Metro 界面, 所以桌面与 Metro 切换频繁.

大部分兼容 Windows 7 的程序在 Windows 8 中使用没有任何问题, 包括驱动.

Metro 界面果然还是适合平板电脑使用, 如果只有鼠标操作起来不太方便, 正式版的时候应该会区分 PC 与平板电脑.

系统速度比 Windows 7 快了很多. 最直观的感受是——UAC 提权窗口会在双击程序后立刻弹出, 而不是像 Windows 7 那样要等几秒钟 (也可能是我没有安装杀毒软件的关系 :-|).

资源浏览器使用了 Ribbon 界面, 正如之前的某些评论那样, 显得繁杂而不友好.

[![Windows 8 Explorer](//img.beamnote.com/2011/windows8-explorer.jpg)](//img.beamnote.com/2011/windows8-explorer.jpg)

资源管理器按 F11 可以全屏, 但是会覆盖开始菜单条.

复制 / 移动 / 删除界面会显示速度量表, 尽管对速度没有帮助, 至少看起来很炫.

[![Windows 8 Copy](//img.beamnote.com/2011/windows8-copy.png)](//img.beamnote.com/2011/windows8-copy.png)

系统自带了微软拼音输入法, 当然需要手动添加了. 看起来 Windows 8 提供了新的输入法注册接口, 现有的第三方输入可以在安装时自动添加到输入法列表中, 但如果你不小心把它从列表中删除了, 是没有任何方法恢复进去的——除非重新安装输入法.

另外, 谷歌拼音输入法似乎与 Metro 界面不兼容, 只能在桌面使用. 不知道其它输入法的表现情况.

最后说一个最神奇的设计: 多系统切换.

系统启动时一般会进入一个小的 Boot 程序, 列出系统清单让用户选择. 但 Windows 8 Developer Preview 会预先进入 Windows RE 之类的小型系统, 完全加载之后才会显示系统清单. 如果选择了 Windows 8, 那么可以立即进入系统; 但如果选择其它系统的话, 会立刻重启 (什么? 重启? \!). 重启之后, 才会进入你选择的系统. 我猜测 Windows 8 此时会设置一个标记, 下次启动时进入 Windows RE 之前就切换到目标系统完成引导.

虽然说可以看到漂亮的高解析度引导界面, 但这样影响速度真的没问题么?
