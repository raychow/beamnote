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
<table>
<tbody>
<tr>
<th>Prerequisite</th>
<th>Debian</th>
<th>Suse</th>
<th>Red Hat</th>
<th>OS X (via MacPorts)</th>
<th>Fedora</th>
<th>NetBSD</th>
</tr>
<tr>
<td>asciidoc</td>
<td>asciidoc</td>
<td>asciidoc</td>
<td>asciidoc</td>
<td>asciidoc</td>
<td>asciidoc</td>
<td>?</td>
</tr>
<tr>
<td>bash</td>
<td>bash</td>
<td>bash</td>
<td>?</td>
<td>bash</td>
<td>?</td>
<td>bash</td>
</tr>
<tr>
<td>binutils</td>
<td>binutils</td>
<td>binutils</td>
<td>binutils</td>
<td>binutils</td>
<td>binutils</td>
<td>?</td>
</tr>
<tr>
<td>bzip2</td>
<td>bzip2</td>
<td>bzip2</td>
<td>bzip2</td>
<td>bzip2</td>
<td>bzip2</td>
<td>?</td>
</tr>
<tr>
<td>fastjar</td>
<td>fastjar</td>
<td>fastjar</td>
<td>libgcj</td>
<td>fastjar</td>
<td>libgcj</td>
<td>?</td>
</tr>
<tr>
<td>flex</td>
<td>flex</td>
<td>flex</td>
<td>?</td>
<td>flex</td>
<td>flex</td>
<td>?</td>
</tr>
<tr>
<td>git</td>
<td>git-core</td>
<td>git-core</td>
<td>?</td>
<td>?</td>
<td>?</td>
<td>?</td>
</tr>
<tr>
<td>g++</td>
<td>g++</td>
<td>gcc-c++</td>
<td>gcc-c++</td>
<td>?</td>
<td>gcc-c++</td>
<td>?</td>
</tr>
<tr>
<td>gcc</td>
<td>gcc</td>
<td>gcc</td>
<td>gcc</td>
<td>?</td>
<td>gcc</td>
<td>?</td>
</tr>
<tr>
<td>getopt</td>
<td>util-linux</td>
<td>util-linux</td>
<td>?</td>
<td>getopt</td>
<td>?</td>
<td>getopt</td>
</tr>
<tr>
<td>GNU awk</td>
<td>gawk</td>
<td>gawk</td>
<td>gawk</td>
<td>gawk</td>
<td>gawk</td>
<td>?</td>
</tr>
<tr>
<td>gtk2.0-dev</td>
<td>libgtk2.0-dev</td>
<td>?</td>
<td>gtk2-devel</td>
<td>gtk2</td>
<td>gtk2-devel</td>
<td>?</td>
</tr>
<tr>
<td>intltool-update</td>
<td>intltool</td>
<td>intltool</td>
<td>intltool</td>
<td>intltool</td>
<td>intltool</td>
<td>?</td>
</tr>
<tr>
<td>jikes</td>
<td>—</td>
<td>jikes</td>
<td>?</td>
<td>jikes</td>
<td>—</td>
<td>?</td>
</tr>
<tr>
<td>libz, libz-dev</td>
<td>zlib1g-dev</td>
<td>zlib-devel</td>
<td>zlib-devel</td>
<td>zlib</td>
<td>zlib-devel</td>
<td>?</td>
</tr>
<tr>
<td>make</td>
<td>make</td>
<td>make</td>
<td>?</td>
<td>gmake</td>
<td>make</td>
<td>gmake</td>
</tr>
<tr>
<td>ncurses</td>
<td>libncurses5-dev</td>
<td>ncurses-devel</td>
<td>ncurses-devel</td>
<td>ncurses</td>
<td>ncurses-devel</td>
<td>?</td>
</tr>
<tr>
<td>openssl/ssl.h</td>
<td>libssl-dev</td>
<td>libopenssl-devel</td>
<td>openssl-devel</td>
<td>openssl</td>
<td>openssl-devel</td>
<td>?</td>
</tr>
<tr>
<td>patch</td>
<td>patch</td>
<td>patch</td>
<td>?</td>
<td>patchutils</td>
<td>patch</td>
<td>?</td>
</tr>
<tr>
<td>perl-ExtUtils-MakeMaker</td>
<td>perl-modules</td>
<td>perl-ExtUtils-MakeMaker</td>
<td>perl-ExtUtils-MakeMaker</td>
<td>p5-extutils-makemaker</td>
<td>perl-ExtUtils-MakeMaker</td>
<td>?</td>
</tr>
<tr>
<td>python2.6-dev</td>
<td>python2.6-dev</td>
<td>python-devel</td>
<td>?</td>
<td>python26</td>
<td>?</td>
<td>?</td>
</tr>
<tr>
<td>rsync</td>
<td>rsync</td>
<td>rsync</td>
<td>?</td>
<td>rsync</td>
<td>rsync</td>
<td>?</td>
</tr>
<tr>
<td>ruby</td>
<td>ruby</td>
<td>ruby</td>
<td>?</td>
<td>ruby</td>
<td>ruby</td>
<td>?</td>
</tr>
<tr>
<td>sdcc</td>
<td>sdcc</td>
<td>sdcc</td>
<td>sdcc</td>
<td>sdcc</td>
<td>sdcc</td>
<td>?</td>
</tr>
<tr>
<td>unzip</td>
<td>unzip</td>
<td>unzip</td>
<td>?</td>
<td>unzip</td>
<td>unzip</td>
<td>?</td>
</tr>
<tr>
<td>wget</td>
<td>wget</td>
<td>wget</td>
<td>wget</td>
<td>wget</td>
<td>wget</td>
<td>?</td>
</tr>
<tr>
<td>working-sdcc</td>
<td>—</td>
<td>?</td>
<td>?</td>
<td>?</td>
<td>—</td>
<td>?</td>
</tr>
<tr>
<td>xgettext</td>
<td>gettext</td>
<td>?</td>
<td>?</td>
<td>gettext</td>
<td>gettext</td>
<td>?</td>
</tr>
<tr>
<td>xsltproc</td>
<td>xsltproc</td>
<td>libxslt</td>
<td>?</td>
<td>libxslt</td>
<td>libxslt</td>
<td>?</td>
</tr>
<tr>
<td>zlib, zlib-static</td>
<td>zlib1g-dev</td>
<td>zlib-devel</td>
<td>?</td>
<td>?</td>
<td>?</td>
<td>?</td>
</tr>
</tbody>
</table>
下列依赖包不能被 `make config` 检查, 应保证它们已被安装:
<table>
<tbody>
<tr>
<th>Package</th>
<th>Prerequisite</th>
<th>Debian</th>
<th>Suse</th>
<th>Red Hat</th>
<th>OS X</th>
<th>Fedora</th>
<th>NetBSD</th>
</tr>
<tr>
<td>intltool</td>
<td>[Perl] XML::Parser</td>
<td>libxml-parser-perl</td>
<td>?</td>
<td>perl-XML-Parser</td>
<td>?</td>
<td>?</td>
<td>?</td>
</tr>
</tbody>
</table>

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
* * 状态表示选中的包将被编译, 但并不包含入固件. 可以在使用过程中使用 `opkg` 命令安装此包;
* N 状态表示选中的包将不会被编译.

## 四、进行编译

虽然直接执行 `make` 就可以开始编译, 但出于各种原因编译很难一次就顺利结束, 因此建议使用以下命令以查看详细的编译信息:

{% codeblock lang:shell %}
make V=s
{% endcodeblock %}

最容易碰到的问题和编译本身没有关系, 由于编译过程中需要在网上下载其它人开发之组件的源码, 常常会由于文件已被删除或被墙的关系而失败. 失败后可以在输出信息的最后几行找到需要下载的文件, 翻墙或自行搜索此文件, 放置在 `dl` 目录下, 重新执行编译即可.

编译需要数十分钟时间, 慢慢等待吧.
