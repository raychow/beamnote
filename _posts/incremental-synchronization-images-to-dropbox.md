---
title: 增量同步网站图片到 Dropbox
tags:
  - Bash
  - Dropbox
  - VPS
  - 网站
id: 470
categories:
  - 作品
date: 2014-02-12 09:30:52
---

换了 VPS 之后我浅浅的探索了一下, 其实与虚拟主机最大的不同也就是多出那些给电脑装机一般的重复性基础工作, 鉴于我入门级的使用水平, 能自由发挥的余地相当的少. 搭 VPS 的过程参考了 [Linode 的知识库](https://library.linode.com/getting-started), 以及这位仁兄的[学习之路](http://cnzhx.net/blog/road-to-vps/), 有空的话我也整理一份自用兼供参考.

[![增量同步网站图片到 Dropbox](http://img.beamnote.com/2014/sync-images-to-dropbox.png)](http://img.beamnote.com/2014/sync-images-to-dropbox.png)<!-- more -->

网站呢最重要的就是数据了, 很多 VPS 不像虚拟主机, 每天都有备份的, 出现问题能够自助连上备份服务器寻回档案, 因此 VPS 使用者一定要做好备份工作. 备份当然不能放在本机, 否则主机坏了备份丢失岂不是白忙活; 现在云服务这么发达, 备份到云端成了不错的选择. Dropbox 作为云服务中经历多年风雨洗礼的佼佼者一直广受推崇, 得益于其 API, 我们也能将网站资料备份到 Dropbox 之中.

在使用虚拟主机的时候, 我就有用 [BackWPup](http://wordpress.org/plugins/backwpup/) 这款 WordPress 插件, 它能定时将你的数据库与网站文件备份到 Dropbox、Amazon S3、SugarSync 等一系列云服务中, 在出现异常的时候还能够发出邮件通知, 算是十分贴心了. 但这款插件设置选项有一些儿多, 而且有时会出现好几分钟都不能完成备份而报错的意外, 尚不明确是主机问题还是插件问题; 另外, 这款插件在 WordPress 的后台地方留下了很多链接, 看着很烦人.

现在既然用 VPS, 那就用 VPS 的方式去实行备份吧. 数据库与网站文件备份到 Dropbox 自不必再说, 不少网站都提供了方法, 但全盘压缩备份的方法对于图片来说有些不合适了, 毕竟一个网站的图片压缩下来至少也有数十 MB, 保留几十天备份的话就要在 Dropbox 里占用数 GB 空间, 对于那些动不动就扩容 2TB、3TB 的国内网盘来说简直是九牛一毛, 但对于寸土寸金的 Dropbox 来说实在是太浪费了. 而且图片这种文件基本上不会修改, 在 Dropbox 储存一对一的备份是完全可以的.

本着这种想法, 我又写起了很少接触的 Bash 脚本, 无奈水平太差, 差点打算放弃用 Python 来写, 但最后查查补补倒也折腾出来了. 这段脚本调用了 Bash 脚本 Dropbox Uploader, 通过 Cron 每天定期执行, 检查上次同步以来被添加或修改的文件, 并同步至 Dropbox. 如果有文件同步失败则中止同步, 等待下一次 Cron Job 执行再做尝试.

Dropbox Uploader 请在这里下载: [https://github.com/andreafabrizi/Dropbox-Uploader](https://github.com/andreafabrizi/Dropbox-Uploader)

将 dropbox_uploader.sh 放到合适的位置 (我放在了 `/usr/local/sbin` 中) 并赋予执行权限, 运行这个脚本, 它会提示你[在此](https://www.dropbox.com/developers/apps/create)创建 Dropbox API 并将 API 资料填入其中. 执行完成后, 你可以尝试执行 `./dropbox_uploader.sh upload file /` 来检查 Uploader 是否正常工作.

然后, 将以下代码根据实际情况修改后保存为 Bash 脚本.

{% codeblock lang:shell %}
IMG_DIR=/path/to/public_html    # 图片 (文件) 所在目录
TOUCH_FILE=.backup              # 被 touch 的文件, 指示了最后同步的图片 (文件) 的时间

cd $IMG_DIR
FIND_NEWER=
if [[ -f $TOUCH_FILE ]]; then
  FIND_NEWER="-newer ${TOUCH_FILE}"
fi

find . $FIND_NEWER -type f -printf "%T@ %p\n" | sort -n | sed "s/^[^ ]* //" |
{
  while read file;
  do
    /usr/local/sbin/dropbox_uploader.sh upload "${file}" "/img/${file}";
    if [[ $? -ne 0 ]]; then
      exit 1
    fi
    touch -r $file $TOUCH_FILE
  done
}
{% endcodeblock %}

并在 Crontab 中为此脚本设置合适的执行时间, 例如在每天三点执行备份:

{% codeblock lang:shell %}
0 3 * * * /usr/local/sbin/backup_img.sh
{% endcodeblock %}

可以先手动执行一次检查错误, 如果一切正常, 第一次运行时会将所有图片 (文件) 同步到 Dropbox, 耗时较长请耐心等待. 其实, 这段脚本不一定必须用来同步文件, 同步服务器文件也是可以的, 只是要同步的文件数量多时速度比较慢.
