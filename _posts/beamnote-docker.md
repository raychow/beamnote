---
title: 光线誌 with Docker
tags:
  - Docker
  - 博客
categories:
  - 分享
date: 2017-03-02 22:52:31
---

Docker 是现在炙手可热的轻量级虚拟化技术, 它在本月将迎来四周岁的生日. 最近我借着翻译集团 Docker 文档的机会, 对 Docker 进行了学习, 便计划着重新动工这个年久失修的博客.

[![Docker](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2017/beamnote-docker.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2017/beamnote-docker.png)<!-- more -->

Docker 能带给个人博客什么好处呢? 我想了想, 大概有这几点:

* 最主要的, 是 Docker 镜像开箱即用的特性, 它大大简化了网站的部署过程.

  + 在以前, 即使是搭建最简单的 LAMP 架构的网站, 也免不了从装机开始, 用软件包管理器安装 Apache, MySql, PHP 并一一配置, 接着要恢复网站应用的配置和数据. 由于操作是纯手工进行的, 十分耗时耗力, 不仅难以避免错误, 也不利于运维工作的复盘或审计; 对于个人网站来说, 每次搭建网站都要去网上搜各种教程, 即使是完完整整的记录下配置步骤, 也可能由于系统不同, 或是应用有升级等原因, 导致步骤不能完整重现.
  + 如果使用 Docker, 那将是另一种情形了: 我们可以从 Docker Hub 上搜寻市场上的镜像, 很多镜像都是可以直接运行使用的; 如果市场上的镜像不能满足要求, 或是有安全性的担忧, 也可以选择自行编译镜像 (当然最好是通过 `Dockerfile` 来构建), 而且 Docker 允许我们指定基础镜像, 不必从零开始. 这样做的好处是, 如果将应用迁移到另一台不同的主机上, 由于 Docker 提供了应用与系统之间的抽象层次, 使得我们可以不配置或少量配置应用, 就能在另一台主机上启动起来 (感觉像是某种语言宣传的[一次编写, 到处运行](https://zh.wikipedia.org/wiki/%E4%B8%80%E6%AC%A1%E7%BC%96%E5%86%99%EF%BC%8C%E5%88%B0%E5%A4%84%E8%BF%90%E8%A1%8C) \[误\]).

* Docker 占用资源少.

  Docker 的容器技术比普通虚拟机快得多, 几乎感受不到 Docker 中运行的应用在速度上的差别, 内存占用也相当小, 即使是一个配置较差的 VPS, 也能顺畅的运行 Docker.

* 运行在 Docker 中的应用较为安全.

  Docker 提供的容器具有一定的隔离能力, 容器与宿主机之间, 容器与容器之间基本上是不能感知的. 例如:

  + 尽管容器是以 root 权限启动的, 但 Docker 限制了容器中 root 用户的 Capability, [容器中的应用只能执行一些低特权的指令](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities).
  + Docker 使用 CGroup 作资源使用的限制, CGroup 能够限制恶意进程抢占资源.
  + Docker 使用 namespace 来隔离资源, 容器不能访问其它 namespace 的资源; 例如 Docker 对容器的 PID 进行了隔离, 容器内部看到的 PID 为 1 的进程, 在容器外部会是另一个 PID. Docker 已提供了附表中的 namespace 隔离.

     > 需要说明的是, 当前版本 (1.13.1) 的 docker 默认没有开启 user namespace 的隔离, 一些 linux 发行版甚至默认没有开启对 user namespace 的支持. 要在 docker 上隔离 user namespace, 可以参考 [Docker安全之用户资源隔离](http://www.zimug.com/453.html).

  + Docker 还可以与 SELinux 结合, 实现对权限更加细致的控制.

  值得注意的是, Docker 的安全只是相对于应用直接在宿主机中运行而言的, Docker 的安全性仍存在很多质疑, 并不推荐在 Docker 中运行不明来源的程序,

* Docker 是免费的.

  开源, 免费, 想用就用.

由此, 博客 VPS 上几乎所有的服务都运行在了 Docker 中, 结合 Docker Compose, 实现一整套应用一键启停和自动重启. 接下来的文章将记录下现在的架构.

附: Docker 的 namespace 隔离:

 namespace  | 引入支持的 Linux 内核版本 | 被隔离的资源              | 隔离效果
------------|-------------------------|--------------------------|--------------------------------------------------------------------
 mount      | Linux 2.4.19            | 文件系统挂载点            | 容器内部只能访问指定的文件系统, 且结构与宿主机无关
 uts        | Linux 2.6.19            | 主机名与域名              | 每个容器有独立的主机名与域名
 ipc        | Linux 2.6.19            | 进程间通讯资源, 如共享内存 | 只有同一个 ipc 内的进程才能互相通信
 pid        | Linux 2.6.24            | 进程 id                  | 每个容器都有独立的进程 id, 容器内每个进程的内部和外部 pid 不一样
 network    | Linux 2.6.24 - 2.6.29   | 网络                     | 每个容器都有独立的网络设备, 网络栈, 端口等
 user       | Linux 2.6.23 - 3.8      | 用户和用户组              | 每个容器内进程的用户和用户组 id 与容器外不同

