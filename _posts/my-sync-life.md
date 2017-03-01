---
title: 我的同步生活
tags:
  - Google
  - 同步
id: 189
categories:
  - 分享
date: 2010-06-11 14:24:54
---

21 世纪是信息化的时代, 网络在我国已经普及. 虽然我们的网速比较慢, 也要物尽其用.

平时有很多东西需要记录了, 例如日历、联系人, 我们已经有了手机, 做这些工作很方便. 但如果手机不慎丢失或者打算换新手机, 记录的资料就有可能丢失, 因为这些内容只存放在手机记忆体中.

[![同步](//img.beamnote.com/2010/my-sync-life.png)](//img.beamnote.com/2010/my-sync-life.png)<!-- more -->

如果将这些内容存放在网路并定期同步, 不仅解决了资料可能丢失的问题, 也能够通过网路这个平台同步到所有支持的设备上.

Google, 这个频繁出现在我博客的品牌, 今天又一次成为了主角. Google 提供了一系列的服务, 包括存放日历、联系人、笔记等资料的平台, 它们就是 [Google 日历](https://www.google.com/calendar/)、[Google 通讯录](https://www.google.com/contacts)、[Google 笔记本](https://www.google.com/notebook/). Google 也提供了与移动设备同步的方法, 下表列出了各平台的支持情况:
<table>
<thead>
<tr>
<th>平台</th>
<td>BlackBerry</td>
<td>iPhone</td>
<td>Nokia S60</td>
<td>SyncML</td>
<td>Windows</td>
</tr>
</thead>
<tbody>
<tr>
<td>Gmail 同步</td>
<td></td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td></td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
</tr>
<tr>
<td>通讯录同步</td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
</tr>
<tr>
<td>日历同步</td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td></td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
</tr>
<tr>
<td>Push 支持</td>
<td></td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td></td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
</tr>
<tr>
<td>管理</td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
<td></td>
<td>![](https://www.google.com/images/icons/check-14x13.gif)</td>
</tr>
</tbody>
</table>
Google 笔记本是 Google 已经停止开发的一个项目, 目前已经能够满足记录需求. 平时有什么中意的文字片段, 直接粘贴到笔记本中即可.

下面说一下诺基亚 S60 系列同步的方法.

**在开始之前:
*** 强烈建议您通过诺基亚 PC 套件或者其它管理软件备份电话中的资料.
* 请确定您的电话与 Mail for Exchange 相容, 并且是[最新版本](http://store.ovi.com/content/28466).

要为 Google 同步配置 Mail for Exchange, 请进行以下操作:

1. 运行 **Mail for Exchange** 应用程序;
2. 提示创建新配置文件时选择 **是**;

![](https://www.google.com/help/hc/images/sync_147951d_sync_create_en.gif)3. 按以下步骤编辑配置文件:
![](https://www.google.com/help/hc/images/sync_147951b_sync_connections_en.gif)

* 连接

    * Exchange 服务器:  m.google.com
    * 安全连接: 是
    * 接入点: 运营商提供的互联网接入点
    * 漫游时同步:  根据需要选择
    * 使用默认端口:  是

* 证书

    * 用户名: 您的 Gmail 地址, 例如 jon@gmail.com
    * 密码: 您的 Google 帐户密码
    * 域: 留空

* 同步日程

    * 决定同步进行的时间. 选择 **始终打开** 将保证您的数据是最新的, 但也会消耗更多电能.

* 日历

    * 同步日历:  是 或 否
    * 同步日历回溯:  根据需要选择
    * 初次同步:  决定您希望在第一次同步时保留电话中已有的日历还是从 Google 日历中全盘替换.

* 任务

    * 同步任务:  否 (Google 同步尚不支持)

* 联系人

    * 同步联系人:  是 或 否
    * 初次同步: 决定您希望在第一次同步时保留电话中已有的联系人还是从 Google 通讯录中全盘替换.

* 电邮

    * 同步电子邮件:  是 或 否
    * 电子邮件地址:  使用刚刚填写的用户名地址
    * 显示新邮件弹出窗口:  是 或 否
    * 使用签名:  是 或 否
    * 签名:  根据需要选择
    * 发送邮件时:  立即发送 或 只在下次同步时发送
    * 同步消息回溯:  根据需要选择

P.s. SkyDrive 的上传程序升级了, 现在使用 SilverLight 构建, 方便很多.
