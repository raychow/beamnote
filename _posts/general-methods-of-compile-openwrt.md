---
title: 编译 OpenWrt 的一般方法
tags:
  - OpenWrt
  - 路由器
id: 433
categories:
  - 分享
date: 2013-03-05 17:15:04
---

OpenWrt 是一个开源的、可自由定制的适合嵌入式设备的 Linux 发行版, 其起源得益于当初 Linksys 释出的路由器源码. 现在, OpenWrt 可以运行在相当多类型的路由器上 (涵盖了 Atheros、Broadcom 等主流方案和其它少数非主流方案), 如果你买Ｘ联、Ｘ星等国产货, 或者 Buffalo、Netgear 等外国货, 有不少型号都能运作 OpenWrt, 让你的路由器瞬间变为可自由操控的设备.

要获得 OpenWrt 二进制文件, 除了寻找官方或私人编译之固件, 更可以自行编译. 自行编译的好处在于可自行定制组件, 而且某些组件对版本要求比较严格, 老版本的固件 (甚至是前一天刷入的) 往往无法安装官网提供的组件, 自行编译就不存在这样的问题; 另外, 还可将配置写入固件, 这样刷好机后无需重新配置.

[![OpenWrt](//img.beamnote.com/2013/openwrt.jpg)](//img.beamnote.com/2013/openwrt.jpg)<!-- more -->

编译 OpenWrt 的一般方法可以总结如下:

## 一、准备编译环境

OpenWrt 需要在主流的 GNU/Linux 发行版下编译, 以 Ubuntu 为示例: 安装好 Ubuntu——无论是安装在实体机, 还是在虚拟机 (但虚拟机下的 Ubuntu 在与路由器进行 TFTP 通讯时需要设定桥接连接网路) 均可, 并安装编译 OpenWrt 所需的依赖包.

首先安装的是版本管理和编译工具:

{% codeblock lang:shell %}
sudo apt-get update
sudo apt-get install subversion build-essential
{% endcodeblock %}

此外, 根据系统的不同, 还要安装不同的软件包, 以 Ubuntu 12.04 LTS 为例, 应输入:

{% codeblock lang:shell %}
sudo apt-get install git-core libncurses5-dev zlib1g-dev gawk flex quilt libssl-dev xsltproc libxml-parser-perl
{% endcodeblock %}

具体软件包的命名, 可以在下面的表格中找到:

| Prerequisite            | Debian          | Suse                    | Red Hat                 | OS X (via MacPorts)   | Fedora                  | NetBSD |
|-------------------------|-----------------|-------------------------|-------------------------|-----------------------|-------------------------|--------|
| asciidoc                | asciidoc        | asciidoc                | asciidoc                | asciidoc              | asciidoc                | ?      |
| bash                    | bash            | bash                    | ?                       | bash                  | ?                       | bash   |
| binutils                | binutils        | binutils                | binutils                | binutils              | binutils                | ?      |
| bzip2                   | bzip2           | bzip2                   | bzip2                   | bzip2                 | bzip2                   | ?      |
| fastjar                 | fastjar         | fastjar                 | libgcj                  | fastjar               | libgcj                  | ?      |
| flex                    | flex            | flex                    | ?                       | flex                  | flex                    | ?      |
| git                     | git-core        | git-core                | ?                       | ?                     | ?                       | ?      |
| g++                     | g++             | gcc-c++                 | gcc-c++                 | ?                     | gcc-c++                 | ?      |
| gcc                     | gcc             | gcc                     | gcc                     | ?                     | gcc                     | ?      |
| getopt                  | util-linux      | util-linux              | ?                       | getopt                | ?                       | getopt |
| GNU awk                 | gawk            | gawk                    | gawk                    | gawk                  | gawk                    | ?      |
| gtk2.0-dev              | libgtk2.0-dev   | ?                       | gtk2-devel              | gtk2                  | gtk2-devel              | ?      |
| intltool-update         | intltool        | intltool                | intltool                | intltool              | intltool                | ?      |
| jikes                   | —               | jikes                   | ?                       | jikes                 | —                       | ?      |
| libz, libz-dev          | zlib1g-dev      | zlib-devel              | zlib-devel              | zlib                  | zlib-devel              | ?      |
| make                    | make            | make                    | ?                       | gmake                 | make                    | gmake  |
| ncurses                 | libncurses5-dev | ncurses-devel           | ncurses-devel           | ncurses               | ncurses-devel           | ?      |
| openssl/ssl.h           | libssl-dev      | libopenssl-devel        | openssl-devel           | openssl               | openssl-devel           | ?      |
| patch                   | patch           | patch                   | ?                       | patchutils            | patch                   | ?      |
| perl-ExtUtils-MakeMaker | perl-modules    | perl-ExtUtils-MakeMaker | perl-ExtUtils-MakeMaker | p5-extutils-makemaker | perl-ExtUtils-MakeMaker | ?      |
| python2.6-dev           | python2.6-dev   | python-devel            | ?                       | python26              | ?                       | ?      |
| rsync                   | rsync           | rsync                   | ?                       | rsync                 | rsync                   | ?      |
| ruby                    | ruby            | ruby                    | ?                       | ruby                  | ruby                    | ?      |
| sdcc                    | sdcc            | sdcc                    | sdcc                    | sdcc                  | sdcc                    | ?      |
| unzip                   | unzip           | unzip                   | ?                       | unzip                 | unzip                   | ?      |
| wget                    | wget            | wget                    | wget                    | wget                  | wget                    | ?      |
| working-sdcc            | —               | ?                       | ?                       | ?                     | —                       | ?      |
| xgettext                | gettext         | ?                       | ?                       | gettext               | gettext                 | ?      |
| xsltproc                | xsltproc        | libxslt                 | ?                       | libxslt               | libxslt                 | ?      |
| zlib, zlib-static       | zlib1g-dev      | zlib-devel              | ?                       | ?                     | ?                       | ?      |

下列依赖包不能被 `make config` 检查, 应保证它们已被安装:

| Package  | Prerequisite       | Debian             | Suse | Red Hat         | OS X | Fedora | NetBSD |
|----------|--------------------|--------------------|------|-----------------|------|--------|--------|
| intltool | [Perl] XML::Parser | libxml-parser-perl | ?    | perl-XML-Parser | ?    | ?      | ?      |

## 二、获取源代码

一般我们只需要主干版本的源码, 除非你有别的需求. 假设需要将代码下载到 `~/openwrt` 中:

{% codeblock lang:shell %}
mkdir ~/openwrt
cd ~/openwrt
svn co svn://svn.openwrt.org/openwrt/trunk/
{% endcodeblock %}

接着下载并安装 [feeds](http://wiki.openwrt.org/doc/devel/feeds):

{% codeblock lang:shell %}
cd ~/openwrt/trunk
./scripts/feeds update -a
./scripts/feeds install -a
{% endcodeblock %}

使用其中任何一个命令来检查依赖关系:

{% codeblock lang:shell %}
make defconfig
make prereq
make menuconfig
{% endcodeblock %}

目前, 完整的 OpenWrt 源码会占用数百兆字节空间, 在编译之后占用空间将会增长至数百万兆字节 (GB).

每次编译前, 可以使用以下命令将源码更新到最新:

{% codeblock lang:shell %}
cd ~/openwrt
svn update
cd trunk
./scripts/feeds update -a
./scripts/feeds install -a
{% endcodeblock %}

## 三、编译配置

在 OpenWrt 目录下运行 `make menuconfg`, 便打开了 OpenWrt Buildroot 配置接口, 在这里可以选择目标平台、Toolchain 版本以及所需包含的软件包.

在界面中, 按空格可以将软件包在 Y (快捷键 Y) 、* (快捷键 M) 、N (快捷键 N) 三个状态中切换:

* Y 状态表示选中的包将被编译并包含入固件中;
* \* 状态表示选中的包将被编译, 但并不包含入固件. 可以在使用过程中使用 `opkg` 命令安装此包;
* N 状态表示选中的包将不会被编译.

## 四、进行编译

虽然直接执行 `make` 就可以开始编译, 但出于各种原因编译很难一次就顺利结束, 因此建议使用以下命令以查看详细的编译信息:

{% codeblock lang:shell %}
make V=s
{% endcodeblock %}

最容易碰到的问题和编译本身没有关系, 由于编译过程中需要在网上下载其它人开发之组件的源码, 常常会由于文件已被删除或被墙的关系而失败. 失败后可以在输出信息的最后几行找到需要下载的文件, 翻墙或自行搜索此文件, 放置在 `dl` 目录下, 重新执行编译即可.

编译需要数十分钟时间, 慢慢等待吧.
