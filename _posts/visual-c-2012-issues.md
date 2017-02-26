---
title: 我遇到的 Visual C++ 2012 问题集锦
tags:
  - Visual C++
  - 编程
id: 447
categories:
  - 分享
date: 2013-05-27 14:33:48
---

Visual C++ 2012 作为 Visual Studio 2012 的一部分, 尽管发布了大半年、出过两次 Update, 还是有一些问题没有被修正. 所幸的是, 我遇到的这些问题都有非官方的解决方式, 相信有不少同学也会遇到这些问题, 因此在这里记录下来.

[![](http://img.beamnote.com/2013/visual_studio_2012.png)](http://img.beamnote.com/2013/visual_studio_2012.png)<!-- more -->

## 一、无法在属性窗口中输入中文

谷歌拼音输入法的用户无法在 IDE 的属性窗口中输入中文, 各种方法尝试无果, 无奈只能从其它窗口复制粘贴; 后来无意中按下 `Shift + Space`, 输入面板居然就…出现了. Orz…

其它输入法 (包括 Windows 自带输入法) 没有这种问题.

## 二、发布的程序无法在 Windows XP 中运行

微软一心想要让 Windows XP 早些儿消失, Visual Studio 2012 的多数套件似乎都放弃了对 Windows XP 的支持. 如果将 Visual C++ 2012 编译的程序拿到 Windows XP 中执行, 你会看到一个郁闷的窗口:

[![在 Windows XP 中运行 Visual C++ 2012 开发的程序](http://img.beamnote.com/2013/vc2012_xp.png)](http://img.beamnote.com/2013/vc2012_xp.png)

不是编译链接出错了, 也不是系统出了问题, 而是微软根本没打算让你的程序在叉屁下运行. 后来 Visual C++ 团队博客宣称会提供 Windows XP 的支持, 还好他们的话兑现了. 起初是放出了一个单独的安装包, 但现在集成入了 Visual Studio 2012 Update, 所以你需要安装新版本的 Update, 并在项目属性页中将平台工具集改为「Visual Studio 2012 - Windows XP (v110_xp)」, 这样生成的程序才可以在 Windows XP 中执行.

[![设置 Visual Studio 2012 的平台工具集](http://img.beamnote.com/2013/vc2012_xp_target.png)](http://img.beamnote.com/2013/vc2012_xp_target.png)

顺便提一下, Update 2 包含 Update 1 的所有更新.

## 三、std::thread 的内存泄漏问题

STL 流行三个版本, Visual C++ 一直用的是 P.J. Plauger 的 STL 实现 (你可以通过源文件找到他的注释). thread 是 C++ 11 新标准的一个内容, Visual C++ 2012 的 STL 实现了这个类, 可惜出现了严重问题: 在某些地方哪怕仅仅定义一个 thread  对象, 然后析构, 就会出现恼人的内存泄漏. 如这个反馈[所述](http://connect.microsoft.com/VisualStudio/feedback/details/757212/vs-2012-rc-std-thread-reports-memory-leak-even-on-stack).

{% codeblock lang:cpp %}
std::thread th([](){});
th.join();
{% endcodeblock %}

以上代码放在 CRT 中执行会产生内存泄露, 泄漏来源甚至无从查起, 此问题官方至今未予解决:

{% codeblock lang:cpp %}
Detected memory leaks!
Dumping objects ->
{455} normal block at 0x005AB1E0, 44 bytes long.
Data: < > 01 00 00 00 00 00 00 00 00 00 00 00 0A 00 00 00
Object dump complete.
{% endcodeblock %}

个人推荐使用 [Boost](http://www.boost.org/) 库部分代替 STL 库, 例如, boost::thread 不仅没有上述问题, 还提供比 std::thread 更加强大的功能, 如中断、限时等待等.

## 四、MFC MaskedEdit Control 的问题

这个控件貌似是微软从 BCGSoft 买的, 买就买吧, 还出问题. 同一个窗口如果出现多个掩码编辑框, Tab 顺序为第一个的框就会退化为普通文本框, 就像下图那样:

[![出现问题的 MFC MaskedEdit Control](http://img.beamnote.com/2013/mfcmaskededit.png)](http://img.beamnote.com/2013/mfcmaskededit.png)

我也不知道怎么更好的解决这个问题, 只有多放一个掩码文本框, 将它设为 Tab 第一顺序并且隐藏, 真的很别扭.
