---
title: IPv6 的一点事儿
tags:
  - IPv6
  - 网络
id: 421
categories:
  - 关注
date: 2012-07-27 22:35:10
---

各位好, 真的是好久没写博客了, 上一篇文章还是在近两个月前写的. 现在到了新学校, 每天都比较忙 (其实是 UP Time 很多但是利用率不高), 上午起床后出门直到晚上十点回来, 冲完凉什么的做完了就快十一点了, 剩不了多少时间做其它事情.

[![IPv6 的一点事儿](http://img.beamnote.com/2012/about-ipv6.jpg)](http://img.beamnote.com/2012/about-ipv6.jpg)<!-- more -->

学校有 IPv6 网路, 在此之前我从未接触过, 所以买了一个路由器折腾了一下, 下面是一点体验 (不做任何实际操作上的担保).

## 一、IPv6 与免认证上网

不少学校的 IPv6 网路是免认证的, 本校也是这样, 因此也不会收钱. 但是 IPv6 线路只能访问 IPv6 ready 之网站, 对于那些 IPv4 网站还是要先通过认证才能登上.

幸运的是, 现在已经有一些服务, 让你可以透过 IPv6 线路访问 IPv4 网站:

* [六飞](http://6fei.com.cn/): 是一个 IPv6 这个名词在中国出现后就一直有的服务, 开始就提供 IPv4 访问 IPv6 的软件, 如今还加入了对 IPv6 访问 IPv4 的支持, 但可能要收费;
* [VENO](http://www.veno2.com/): 号称无缝融合 IPv4 与 IPv6, 需要安装虚拟网卡. 美中不足的是我连都连不上 :-? ;
* sixxs.org: 是一个代理网站, 输入想要访问的网址加上 sixxs.org, 例如 [www.google.com.siixs.org](http://www.google.com.siixs.org), 就能打开 Google. 但无法支持一些加密网站;
* [StudioProxy](http://ipv6.studioproxy.com/): 也是一个代理网站, 需要在页面中输入访问的网址;
* 等等.

## 二、IPv6 与路由器

IPv6 没有类似于 IPv4 的 NAT. NAT 用于私有地址与公有地址的转换, 使得一个 IPv4 地址能够连接多台主机, 缓解了 IPv4 地址不足的矛盾; IPv6 设计者认为其拥有足够多的地址, 没有必要采用 NAT.

没有 NAT 对用户最大的影响是, 如果运营商只分配给用户一个不可分割的 IPv6 地址, 用户将只能连接一台电脑 (这仅仅是如果——因为中国的 IPv6 有种没有商用的感觉……).

另外, 现在所有的低端 (家庭级) 路由器都不支持 IPv6, 需要刷入第三方系统才可以.

我用着 BUFFALO 的 WHR-G300N V2, 最大的优点是刷不死; 配置一般般, 4MB 存储空间是个败笔: <del datetime="2012-07-31T10:04:16+00:00">没有足够的空间装下一个完整的 wget, 不能对网页 POST 数据, 所以不能自动进行登录认证了, 这是题外话</del>.

尽管没有足够的空间按装下完整的 wget, 但还有一个不带 SSL 的 wget-nossl 可供选择.

路由器现在跑着 OpenWrt, 可定制性相当高的一个 Linux 系统, 但是 IPv6 配置也很麻烦. 尝试了[《OpenWrt 下单一 IPV6 地址做网关》](http://www.openwrt.org.cn/bbs/forum.php?mod=viewthread&amp;tid=7116)提供的后两种方法, 只有第二种成功了: 让 IPv6 流量从网桥上通过, 相当于放弃了路由功能, 每个接口直接与外网相连.

## 三、IPv6 与科学上网

包括清华大学、北邮在内的知名高校在涉及 IPv6 时都给出了科学上网的暗示, 的确目前 IPv6 科学上网比 IPv4 方便不少.

详情请移步: [IPv6 hosts](http://code.google.com/p/ipv6-hosts/)
