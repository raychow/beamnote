---
title: 光线誌 with Docker 结构篇
tags:
  - Docker
  - 博客
categories:
  - 分享
date: 2017-11-04 17:30:33
---

在光线誌 Docker 化改造之前, 一直采用 WordPress 驱动博客运行. WordPress 作为一个有多年历史的博客 / CMS 系统, 功能相当强大, 是很多人搭建独立博客的第一选择, 但也是一些攻击者们的首选目标.

[![光线誌 with Docker 结构篇](//img.beamnote.com/2017/beamnote-docker-2.png)](//img.beamnote.com/2017/beamnote-docker-2.png)<!-- more -->

受精力所限, 光线誌近来打理的很少, 虽然访客不多, 但从访问日志来看, 每天还是有很多恶意性质的访问. 在恶意访问当中, 对于 WordPress 的试探性攻击占了绝大多数, 例如尝试登录后台, 或是发起 XML-RPC 攻击, 等等. 博客在改造之前, 经常会完全宕掉, 无法连接数据库或是完全不能访问, 是因为经常遭到大量的 XML-RPC 请求, 无端占用 CPU / 网络资源, 处理起来有些恼人.

WordPress 容易遭受攻击与其繁多的功能是脱离不了关系的, 但其中很多功能我并不需要, 只要一个纯粹的博客就好. 与 WordPress 这类需要依靠后台生成内容的博客相对, Jekyll, hexo 等全静态化博客系统正越来越流行, 它们只在文章发布时运行以生成静态资源, 访问时完全不依赖后台服务. 没有了可以直接调用的服务, 博客运行起来将省心不少, 于是我将博客迁移到了 hexo, 开始了 Docker 化改造的第一步.

Docker 有其[最佳实践](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/), 在对博客进行 Docker 化改造时, 建议每个容器只提供一个单一的服务, 类似于时下提倡的微服务, 将一个完整的系统拆分为多个独立的容器, 维护起来更加省心.

目前博客运行的容器为:

* [hexo](https://hexo.io/), 从博客 markdown 源码生成博客页面 (全量 + 增量);
* [webhook](https://github.com/adnanh/webhook), 监听博客源码 git 仓库的变更, 同步更新到本地;
* [isso](https://posativ.org/isso/), 评论服务器, 提供 HTTP 形式的 API, 由博客上的 js 脚本调用, 是目前博客唯一的后台服务;
* nginx, web 服务器, 除了转发 isso 的流量之外, 基本上就是返回静态文件了.
* logrotate, 日志切分;

要管理这四个 Docker 容器其实也是件很麻烦的事情, 使用 docker-compose 可以快速的编排容器, 负责构建, 连接, 启动容器, 并在容器挂掉的时候自动重启. 服务器启动完成之后, docker-compose 就会负责帮我们把博客启动起来, 十分省心.

最近恰逢阿里云双十一, 限定几种规格的云服务器卖的非常便宜 (1 核 1 GB 内存的 ECS 价格为 720RMB/3yr, 2 核 4GB 内存的 ECS 价格为 1500 RMB/3yr), 我将博客搬到了新购的服务器上. 但服务器的带宽只有 1Mbps, 如果要加带宽价格非常高, 由于已经完成了博客的静态化, 实际上带宽问题很好解决了:

1. 将静态资源迁移到阿里云 OSS 等对象存储服务, 为此需要:
    1. 将图片与静态资源全量部署到 OSS 中;
    2. hexo 更新博客后, 将增量内容通过 OSS API 推送过去;
    3. OSS 不支持直接绑定自己的证书, 因此需要使用 OSS 的域名来访问资源, 要将所有资源文件的域名都替换掉;
2. 或者接入 CDN, 静态资源全走 CDN, xhr 请求回源.

CDN 迁移的成本比 OSS 要小, 而且 cdn 一般在各地都由部署节点, 速度上肯定比单一的 OSS 要快. 腾讯云 CDN 每个月都有 10 GB 的流量, 对于我的博客来说基本上不用花钱了😂.

最终, 我的博客结构成了这个样子:

[![光线誌 Docker 化结构](//img.beamnote.com/2017/beamnote-docker-structure.svg)](//img.beamnote.com/2017/beamnote-docker-structure.svg)
