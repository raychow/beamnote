---
title: 使用 Cron jobs 自动备份网站 (二)
tags:
  - 博客
  - 备份
id: 213
categories:
  - 技巧
date: 2010-08-04 13:32:58
---

这一次继续探索备份数据库并发送到指定邮箱.

在 cPanel 后台有一个地址可以直接获取某个数据库的备份, 地址是 `/getsqlbackup/database.sql.gz`, 原以为把这个文件发送到邮箱很简单, 试着直接用 php 实现, 结果不仅要先下载这个文件, 还得用很长的代码把文件大卸八块寄出去, 还是得放弃 php 直接使用脚本.

[![Cron jobs](//img.beamnote.com/2010/cpanel-automatic-backup-chapter-2.png)](//img.beamnote.com/2010/cpanel-automatic-backup-chapter-2.png)<!-- more -->

Linux 下的 Bash 我没用过, 四处搜寻了很多实现自动备份的代码, 碰巧上篇中[流年](http://liunian.info/)同学也提到了一种[方案](http://vastars.info/application/database-backup.html), 我结合刚刚的数据库备份的方法修改了一下, 伪原创的内容就诞生了.

## 一、获得数据库

### 1\. 针对 cPanel 的方法

首先要做的事情, 是下载数据库备份, cPanel 面板使用 wget 轻而易举:
{% codeblock lang:bash %}
#!/bin/sh
cpuser="username" # 登录 cPanel 的帐号
cppass="password" # 登录 cPanel 的密码
dbpath="http://example.com:2082/getsqlbackup/database.sql.gz" # 直接把备份面板的「下载 MySQL 数据库备份」链接复制过来
savepath="database.sql.gz" # 保存的文件名
wget --http-user="$cpuser" --http-password="$cppass" $dbpath -O $savepath
{% endcodeblock %}
`dbpath` 就是 cPanel 中给出的数据库备份地址 (位于 http://www.example.com:2082/frontend/x3/backup/index.html, 与您的 cPanel 主题有关), 直接复制即可.

文件下载到了放置脚本的文件夹, 您可以自己修改.

### 2\. 通用方法

{% codeblock lang:bash %}
#!/bin/bash
dbuser="username"   # 登录数据库的帐号
dbpass="password"   # 登录数据库的密码
dbname="database"   # 数据库的名称
path="database.sql" # 保存的文件名
mysqldump --add-drop-table --opt -u$dbuser -p$dbpass $dbname > $path
gzip $path
{% endcodeblock %}
在进行压缩后, 文件名自动加上 .gz 后缀.

## 二、发送邮件

接下来就是把这个文件发送出去了, 依然有两种方法, 一是使用 Python 发送, 代码多一点 (但是比 php 简单……); 二是使用 mutt, 这东西得主机商安装了才能用 (我自己在 Ubuntu 上装了, 怎样都不能发送附件).
{% codeblock lang:bash %}
#调用 python 脚本发送. sendmail.py 和 backup.sh 放在同一个目录下.
python sendmail.py
#如果空间有 mutt 邮件程序, 可以考虑直接用 mutt 发送, 将上一句屏蔽, 下一句取消屏蔽.
#mutt yourname@example.com -a ./database.sql.gz -s 「Database Backup」
{% endcodeblock %}
使用 Python 发送邮件就直接使用 Vastar 的 sendmail.py 了, 其中附件地址 `file_name` 要和 backup.sh 中指定的文件名保持一致 (./database.sql.gz).

## 三、最后的工作

> 按这里下载: [cPanel](/wp-content/uploads/2010/08/db-backup-1.zip), [通用](/wp-content/uploads/2010/08/db-backup-2.zip)
不要忘记把 backup.sh 权限设定为 700, 否则没有运行的权限.

按照[上一篇](http://raychow.info/2010/cpanel-automatic-backup-chapter-1.html)提到的方法设定 Cron jobs, 但 Command 填写的是 backup.sh 文件, 例如 `/home/username/backup.sh`, 别弄错了.
