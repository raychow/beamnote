---
title: 海盗湾与磁力链接
tags:
  - 下载
  - 磁力链接
  - 网络
id: 407
categories:
  - 关注
date: 2012-02-17 15:15:29
---

海盗湾是一个专门储存、分类和搜寻 BitTorrent (缩写为 BT, 下同)  种子文件的网站, 并自称「世界最大的 BT 种子服务器」, 提供的 BT 种子除了有自由版权的收集外, 也有不少被著作人声称拥有版权的音频、视频、应用软件与电子游戏, 为网路分享与下载的重镇之一. <sup>[[来源]](http://zh.wikipedia.org/zh/%E6%B5%B7%E7%9B%9C%E7%81%A3)</sup>

海盗湾因版权问题多次被瑞典政府起诉, 在九年之间经历过多次追捕与封锁. 最近, 瑞典一家法院再次判处海盗湾的几名管理员有罪入狱. 2 月 14 日, 海盗湾宣布从 2 月 29 日起将不再提供 BT 种子下载, 全面转向磁力链.

[![海盗湾与磁力链接](//img.beamnote.com/2012/the-pirate-bay-and-magnetic-link.jpg)](//img.beamnote.com/2012/the-pirate-bay-and-magnetic-link.jpg)<!-- more -->

在不需要保存数额庞大的种子后, 这个站点的容量将急剧减少, 据统计, 海盗湾整站的一个备份文件尺寸仅有 90 MB, 解压缩后 164 MB, 这一备份文件中包含多达 1643194 个转换后的磁力链接. 也就是说全球最大的资源站点用一张 CD 装也绰绰有余. <sup>[[来源]](http://cnbeta.com/articles/172140.htm)</sup>

磁力链接究竟为何物, 为何能够以一小段文本代替原本庞大的 (一般为数十千字节) BT 种子?

## BitTorrent 协议

BitTorrent 协议是一个网络文件传输协议, 它能够实现点对点文件分享的技术. 比起其他点对点的协议, 它具有多点对多点的特性, 该特性简而言之即为: 下载一档案的人越多, 且下载后, 并继续维持分享 (上传) 的状态就可以成为可让其他人下载的种子文件 (.torrent), 该档案即下载速度越快.

根据 BitTorrent 协议, 文件发布者会根据要发布的文件生成提供一个 .torrent 文件, 即种子文件, 也简称为「种子」.

下载时, BT 客户端首先解析种子文件得到 Tracker 地址, 然后连接 Tracker 服务器. Tracker 服务器回应下载者的请求, 提供下载者其他下载者 (包括发布者) 的 IP. 下载者再连接其他下载者, 根据种子文件, 两者分别告知对方自己已经有的块, 然后交换对方所没有的数据. <sup>[[来源]](http://zh.wikipedia.org/wiki/BitTorrent_(%E5%8D%8F%E8%AE%AE))</sup>

Tracker 服务器在传统的 BT 下载中扮演着重要的角色, 因而成为政府与维权组织封杀的目标. 一旦不能连接 Tracker 服务器, 传统的 BT 下载将无法进行.

## DHT 网路

DHT 网路技术可以在没有 Tracker 服务器的情况下进行 BT 下载.

DHT 全称为分布式哈希表 (Distributed Hash Table), 是一种分布式存储方法. 在不需要服务器的情况下, 每个客户端负责一个小范围的路由, 并负责存储一小部分数据, 从而实现整个 DHT 网络的寻址和存储. 使用支持该技术的 BT 下载软件, 用户无需连上 Tracker 服务器就可以下载, 因为软件会在 DHT 网络中寻找下载同一文件的其他用户并与之通讯, 开始下载任务. <sup>[[来源]](http://zh.wikipedia.org/wiki/BitTorrent_(%E5%8D%8F%E8%AE%AE)#DHT.E7.BD.91.E7.BB.9C)</sup>

## PEX 协议

PEX 是一种增强 BitTorrent 的通信协议, 它允许一组用户或节点合作以迅速而有效的共享指定的文件. PEX 协议能够在一个群 (「群」包含了可以同时下载与上传的主机) 中直接交换群的信息, 而不需要轮询 Tracker 服务器或者 DHT 网络.

PEX 不能自己加入一个群, 而必须使用一个 BT 种子文件或者引导节点 (Bootstrapping node, 为新加入的节点提供初始配置信息) 以找到一个描述了节点信息的 DHT 网络. <sup>[[来源]](http://en.wikipedia.org/wiki/Peer_exchange)</sup>

## 例子

小赵 (用户) 在 A 班 (通过引导节点进入), 她不认识 B 班的小何, 也不认识 C 班的小温. 但小赵认识同班的小王, 而小王认识 B 班的小何, 也可能还认识 C 班的小温, 或者小王仅认识 B 班的小何, 但小何认识 C 班的小温, 而小温又认识同班的所有同学, 结果就是小赵可以「无限」地延伸自己的关系网, 不管怎样, 总有一条沟通途径可以将这些同学联系在一起, 待小赵「认识」了小温后, 他们就可以直接沟通了, 在 P2P 世界里, 就是进行上传与下载.

## 磁力链接

磁力链接 (Magnet URI) 基于文件内容来识别文件, 而不是基于文件的位置或者名称的. 更确切地说, 它是通过文件的散列函数值来识别的.

这个标准的草稿出现于 2002 年, 是为了对 eDonkey2000 的「ed2k:」和 Freenet 的「freenet:」两个URI格式进行「厂商与项目中立化」而制定的. 同时这个标准也尝试紧密地跟进 IETF 官方的 URI 标准. 一个磁力连结可被运行在几乎所有平台上的应用程序们使用以下载一个文件.

因此, 看起来磁力链接与知名 ed2k 协议有些相似, 例如:

`magnet:?xt.1=urn:sha1:YNCKHTQCWBTRNJIV4WNAE52SJUQCZO5C&amp;xt.2=urn:sha1:TXGCZQTH26NL6OUQAJJPFALHG2LTGBC7`

## 总结

磁力链接可是说是「无种子的 BT 下载模式」, 使用文件标识符来确定文件, 比 BT 种子的体积更小. 可以说即使 BT 分享站倒下了, 也根本不会影响原有文件的分发、下载. 要想封锁这种下载方式, 会比封锁 ed2k 下载更加困难.
