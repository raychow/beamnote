---
title: 解决 Qt 中 QFileSystemModel 浏览大量文件时程序失去响应的问题
tags:
  - Qt
  - 编程
id: 480
categories:
  - 技巧
date: 2014-06-27 21:21:05
---

最近在用 Qt 做一个开源项目, 项目中需要显示一个文件与文件夹列表, 这本来是个很简单的要求, Qt 中也提供了一个文件系统模型 `QFileSystemModel`, 此模型与 `QListView` 等控件配合就可以实现目标了.

[![Qt](http://img.beamnote.com/2014/qt.png)](http://img.beamnote.com/2014/qt.png)<!-- more -->

但是, 我在 64 位的 Windows 8.1 下实验发现, `QFileSystemModel` 打开包含了大量文件的文件夹时很容易卡死. 譬如位于固态硬盘的系统的 System32 文件夹, 包含了将近四千个文件 (子文件夹中的文件不做计算), 使用 Qt Creator 使用 MSVC 2013 编译器创建的程序要花费近十秒钟才能恢复响应, 如果直接在 VS 2013 下编译的程序表现会更加糟糕, 花费时间达到了二十余秒. 这么久的时间显然是不可接受的.

经过几个小时的跟踪, 发现问题出在 `QFileSystemModelPrivate::_q_fileSystemChanged` 上, 这个函数根据描述应该是作为线程调用, 但不幸的, 程序经过了某段黑魔法代码后, 由主线程调用了这个函数. 此函数将所有文件的信息读取并缓存, 正是读取图标的过程严重拖慢了整体速度.

奇怪的是, 网上很少人提到了这个问题, 只看到 Stack Overflow 上的[一则回答](http://stackoverflow.com/questions/4055726/qt-qfilesystemmodel-q-filesystemchanged-slot-is-executed-on-the-ui-thread-whic):

> I think the reason for this could be the icons. Within the _q_fileSystemChanged() slot fileInfoGatherer.getInfo() gets called which - among other things - resolves the icons for the paths. In it's current design QFileIconProvider uses QIcon to represent icons and QIcon can only be used in the UI thread. QImage seems to be the only class allowed to use in other threads, but I think it may be to expensive to use QImage in the background thread and convert it to an QIcon in the UI thread.
>
> So it is possible that the platform implementation of QFileIconProvider is slow on network paths under some circumstances and therefore slows down the UI main thread.
>
> I don't know if this is the source of your problem, but at least this should be the reason why _q_fileSystemChanged() is called within the UI thread.

由于用于表示文件图标的 `QIcon` 需要在主线程中使用, `_q_fileSystemChanged` 也被迫执行在了主线程上, 也许以后的 Qt 将会修复这个问题.

目前的权宜之计是, 加快图标的读取速度. 经过多次实验, 我发现读取可执行程序的图标是导致卡顿的主要因素. 既然如此, 干脆不显示程序图标, 速度就能加快很多.

`QFileSystemModel` 使用 `QFileIconProvider` 来读取图标, 我们继承这个类, 重写其中一个用于读取图标的 `icon` 函数:
{% codeblock lang:cpp %}
class MyFileIconProvider
  : public QFileIconProvider
{
public:
    MyFileIconProvider()
        : QFileIconProvider()
    {}

    QIcon icon(const QFileInfo &amp;info) const
    {
        if (info.isExecutable())
        {
            return QIcon();
        }
        return QFileIconProvider::icon(info);
    }
};
{% endcodeblock %}
在传入一个可执行程序的信息时, 函数直接返回了一个空图像.

接着, 继承 `QFileSystemModel` 并设置其 `IconProvider`:
{% codeblock lang:cpp %}
class MyFileSystemModel
    : public QFileSystemModel
{
    Q_OBJECT

public:
    MyFileSystemModel(QObject *parent = 0)
        : QFileSystemModel(parent)
    {
        setIconProvider(&amp;m_myFileIconProvider);
    }

private:
    MyFileIconProvider m_myFileIconProvider;
};
{% endcodeblock %}
`MyFileSystemModel` 在显示可执行文件时, 将使用一个未知文件类型的图标来代替, 如图所示:

[![QFileSystemModel without executable file icon](http://img.beamnote.com/2014/qfilesystemmodel-without-executable-file-icon.png)](http://img.beamnote.com/2014/qfilesystemmodel-without-executable-file-icon.png)

经过实验, Qt Creator 使用 MSVC 2013 编译的程序在两秒内就能打开 System32 文件夹, 速度尚可接受.
