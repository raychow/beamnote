---
title: Y450 拥抱黑苹果 (一)
tags:
  - Mac OS
  - 电脑
id: 176
categories:
  - 分享
date: 2010-05-20 15:44:02
---

最近几日一直在折腾 Mac OS, 让它运行在 Y450 上, 就是所谓的「黑苹果」. 虽然时间花去不少, 现在仍有一些问题无法解决, 但大体上已经正常, 先看看偶的成果 :-).

[![Mac OS X](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/mac-os-x-500.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/2010-05-18_10-48-46.png)<!-- more -->

如果您 (指非苹果电脑使用者) 也想体验 Mac OS 风情的话, 建议您先去 [MacUknow](http://www.macuknow.com)、[netkas.org](http://netkas.org/)、[Macidea](http://www.macidea.com/) 等坛子泡泡, 安装黑苹果的路途可能很漫长, 其中某些步骤也很危险, 建议您具备一定常识之后再进行安装.

Mac OS 是很优秀的一款系统, 不仅速度快, 画面也很耐看. 如果您打算长期使用这个系统, 建议您购买苹果电脑, 省去使用兼容机的一切麻烦.

联想 Y450 作为一款畅销到撞机概率大到无语的神机, 在安装 Mac OS 时优势备显: 在我之前已有无数前人做好安装尝试, 我们只需拮取他们的成果. 如果您正在使用 Y450, 阅读此文可以帮助您最快的安装 Mac OS (不过受某些驱动无解的影响, 对应功能不可用).

## 一、准备

请下载或准备下列内容:

1. Mac OS 系统镜像. 我安装的系统版本是 Mac OS X 10.6.3, 您可以在[此处](http://bbs.pcbeta.com/thread-724423-1-1.html)或[此处](http://bbs.pcbeta.com/thread-726253-1-1.html)找到下载链接;
2. [Java SE Runtime Environment](http://www.greendown.cn/soft/14041.html), HFSExplorer 需要 Java 运行环境;
3. MBR 安装补丁, 帮助 Mac OS 安装在 MBR 分区格式的硬盘上;
4. HFSExplorer, 用于提取 Mac OS 的安装文件;
5. Leopard 安装助手, 用于将 Mac OS 安装文件镜像写入硬盘分区;
6. Bootthink, 用于引导 Mac OS, 同时完成了伪装苹果电脑、加载驱动的工作;
7. 适用于 Y450 with GT130M 的 DSDT 文件, 适用于 Y450 with GT240M 的 DSDT 文件, 请根据您的电脑选择下载;
8. Windows 7 / Vista 安装光盘或 PE, 用于 Mac OS 安装完成后恢复 Windows 的引导.
第 3 ~ 7 条的文件请按此下载: [SkyDrive](http://cid-2343b4049d516106.skydrive.live.com/self.aspx/Ray^4s%20files/Mac^_OS^_X^_10^_6^_3^_Setup^_Tools.zip).

下载完成后, 就可以开始安装系统了. 下面的指南修改自[此处](http://bbs.pcbeta.com/thread-592288-1-1.html).

## 二、分区

在 PC 机上安装 Mac 一般不选择刻盘的方法, 首先 Mac OS 镜像格式为 dmg, Windows 下无法直接刻录需要转换, 其次安装文件较大 (6 个多 G), 需要一张 D9 光盘才能刻录, 失败的可能性高, 需要额外资金支出.

我们使用硬盘安装搞定这些问题, 需要安装与目标系统两个 HFS 分区.

下面列出主要步骤:

1. 右键单击「我的电脑」或「计算机」, 选择「管理」, 在弹出的「计算机管理」窗口左边选择「磁盘管理」, 此时可以看见目前硬盘分区状况;
2. 确定需要分给苹果安装分区与系统分区的大小 (与使用 Windows 的观念不同, 使用 Mac 或 Linux 系统只有一个分区也容易管理). 安装分区依据稍候生成的镜像文件决定, 在本文提供链接下载的版本占用 6.69 GB 空间, 我为它分配了 6.8 GB 空间; 系统分区可以自己决定, 如果安装这个系统仅仅为了尝鲜, 可以分配 15 ~ 20 GB 空间, 如果打算长期使用, 建议分配至少 40 GB 空间.

    1. 对于 Windows Vista / 7 / 2008 用户, 右键单击想要放置苹果系统位置的分区 (此分区要求是 NTFS 格式), 选择「压缩卷」, 输入要分出的大小 (即安装分区 + 系统分区的大小, 如 6.8 GB + 40 GB = 46.8 GB, 46.8 GB * 1024 MB/GB = 47923.2 MB);
    2. 对于 Windows XP / 2003 或更低版本用户, 请使用第三方分区软件完成此操作, 如 Acronis Disk Director Suite;

3. 您将看见分区列表中有一块未分配空间. 右键单击这一部分, 选择「新建简单卷」, 设定大小为 6963 MB (6.8 GB * 1024 MB/GB = 6963.2MB), 分配一个驱动器号, 不要格式化这个卷; 之后, 选择剩下的未分配空间, 新建一个占满剩余空间的简单卷.
[![计算机管理 - 磁盘管理](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/mac-os-mmc.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/mac-os-mmc.png)

至此, 分区工作完成. 如果在系统安装完成后需要删除安装分区, 请确保此分区紧邻想要合并到的分区.

## 三、准备安装镜像

我们刚刚下载的 Mac.OS.X.10.6.3.Retail.dmg 文件包含了系统安装文件、引导、适合 Windows 的驱动, 我们只需要提取安装文件.

1. 如果您没有 Java 的运行环境, 请先安装. 之后, 安装 HFSExplorer;
2. 打开 HFSExplorer, 选择 File - Load file system from file, 打开下载的系统镜像 Mac.OS.X.10.6.3.Retail.dmg;
3. 选择读取 "Mac_OS_X" (Apple_HFS) 分区;
[![Choose Apple Partition Map partition](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/choose-apple-partition-map-partition.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/choose-apple-partition-map-partition.png)
4. 选择 Tools - Create disk image..., 保存提取出的安装镜像, 必须放置在 NTFS 分区;
[![Create disk image](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/create-disk-image.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/create-disk-image.png)
5. 等待写入完成.

## 四、写入安装镜像

1. 以管理员身份运行 Leopard 硬盘安装助手;
2. 镜像文件选择刚才在 HFSExplorer 中保存的安装镜像, 目标分区选择刚刚分配的 6.8 GB 分区 (注意不要选错), 取消「在 boot.ini 中加入 tboot」、「强制加入引导及启动代码」、「PC_EFI V8」三个复选框的选中状态;
3. 单击开始, 程序将会向选择的分区写入安装文件;
4. 耐心等待写入完成, 此时 Windows 会认为程序停止响应, 请不要强制关闭该程序;
5. 完成后, 请检查是否有「Change Partition type to AF: Success」这一行, 如果出现 Failed, 请格式化分区重新开始, 或者使用分区工具将分区格式标示为 AF, HFS 分区应有 AF 标记.
[![Leopard 硬盘安装助手](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/leopard-setup-tools.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/leopard-setup-tools.png)

## 五、修改安装文件

Windows 默认使用 MBR 分区架构, 而 Mac OS 使用 GPT 分区架构, 要想把 MBR 转换为 GPT, 必须删除硬盘上所有分区, 显然比较麻烦. 我们通过修改 Mac OS 系统安装文件, 让其支持 MBR 分区架构.

1. 安装 MacDrive, 这是一个提供 7 天试用的版本 (在我提供的下载中有注册机 :-| );
2. 重新启动计算机, 系统将可以读取 HFS 分区内容; 如果没有看见 Mac OS 安装分区, 请在磁盘管理中为此分区分配一个驱动器号;
3. 将 OSInstall_1063_10D575_deviato.zip 解压:

    * OSIntall 文件放置于 /System/Library/PrivateFrameworks/Install.framework/Frameworks/OSInstall.framework/Versions/A/ (实际上复制与被复制的两个文件相同, 此步骤可以不进行);
    * OSInstall.mpkg 文件放置于 /System/Installation/Packages.

## 六、安装 Bootthink

Bootthink 帮助引导 Mac OS, 安装 Bootthink 之后, Windows 的引导菜单将会增加 Bootthink 一项.

如果 Bootthink 安装失败, 尝试使用 Chameleon 代替; 如果仍然不行, 可能是不完整的 MBR 导致的, Windows XP /2003 用户使用安装光盘的 FixMBR 命令, Windows Vista / 7 / 2008 用户使用 bootrec /fixmbr 命令修复. 即使这样仍然无法进入, 请重新安装 MSDN 版本的 Windows.

## 七、复制 DSDT 文件、kext 驱动

DSDT 文件全称 Differentiated System Description Table (区分系统描述表). DSDT 提供关于基本系统的实现和配置信息. 使用 DSDT 文件，能, 帮助 Mac OS 了解您的电脑，解, 部分兼容、驱动安装的问题.

Y450 的 DSDT 文件可以帮助 Mac OS 识别 Y450 的显卡, 支持睡眠功能、SpeedStep 降频节能技术, 防止 BIOS 重置 (被重置的经典表现就是运行 Mac OS 之后重启联想 LOGO 奏乐).

对于使用 Bootthink 引导的, 将解压的 dsdt.aml 放置于 C:\Darwin\ 目录下.

Mac OS 仅为苹果电脑开发, 所以即使使用了 DSDT 文件, Mac OS 也不能顺利驱动所有硬件, 我们仍然需要额外的 kext 驱动.

鄙人不才, 没有搞定电池问题, Intel Wifi Link 5100 AGN 也属于全球无解状态. 驱动将在下一篇给出, 您也可以自己上网搜寻.

## 八、安装 Mac OS

1. 先进入 BIOS 检查硬盘传输模式是否为 AHCI, 方法为:

    1. 开机时按下 F2, 进入 BIOS 设定;
    2. 按右键, 进入「Advanced」选项卡, 按下键, 移动光标到「SATA Mode Selection」;
    3. 如果为 IDE, 请修改至 AHCI. 这会造成 Windows 无法启动, 请在谷歌[搜索](http://www.google.com/search?hl=zh-CN&amp;newwindow=1&amp;q=Y450+AHCI+%E8%93%9D%E5%B1%8F&amp;aq=f&amp;aqi=&amp;aql=&amp;oq=&amp;gs_rfai=)自行解决.

2. 重新启动计算机, 进入 Bootthink, 按左右键将光标移动至「Mac OS X Install DVD」;
[![Bootthink](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/bootthink.jpg)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/bootthink.jpg)
3. 建议您此时按下 F8, 键入「-x32」, 按下回车. 这样做使得安装器调用 32 位系统内核. 不选择 64 位的原因是很多程序与驱动与 64 位系统不相容, Y450 的网卡驱动就不能工作于 64 位模式;
4. 不出意外将进入语言选择界面, 如果出现五国语言的错误画面 (这就是使用黑苹果最容易遇到的画面了), 请关机, 将上述步骤检查一遍……;
[![Mac OS 选择语言](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/mac-select-language.jpg)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/mac-select-language.jpg)
5. 选择语言之后, 不要急着安装, 先选择顶部菜单的 实用工具 - 磁盘工具:

    1. 在左边的磁盘与分区列表中, 单击之前分出的系统分区, 可以通过分区位置、大小判断;
    2. 在右边选择「抹掉」, 格式选择「Mac OS 扩展 (日志式) 」, 键入名称, 单击右下角的「抹掉」;
    3. 抹掉成功后, 单击左上角的关闭按钮.

6. 出现选择安装目的宗卷时, 单击左下角的自定按钮, 根据自己的喜好选择组件. 打印机驱动占了很大一部分, 建议取消勾选;
7. 选择目的宗卷, 开始安装;
8. 耐心等待安装完成, 去除打印机驱动后需要半个多小时;
9. 安装完成后, 提示 30 秒后重新启动计算机, 抓紧时间打开终端, 将 Windows 分区设定为活动分区:

    ```
    diskutil list (查找 C: 的位置)
    fdisk -e /dev/rdisk0
    f  1 (根据上一步所见, 设置硬盘 0 分区 1 为活动分区, 这里应该含有 Windows 引导信息)
    w
    y
    quit
    ```

如果您来不及完成操作, 可以准备一张 Windows 7 的安装光盘, 选择 修复计算机 - 启动修复, 自动标识活动分区;

进入 PE 使用磁盘工具也可以手工设定活动分区.

> 不完成这个步骤将无法引导计算机

## 九、进入 Mac OS

我们终于可以进入 Mac OS 了, 先打开 Bootthink, 选择 Mac OS 的分区 (别选成了安装分区), 按下 F8, 键入「-x32」, 按下回车.

风火轮转动完成后, 出现欢迎动画. 由于之前已经放置了 DSDT 文件, 所以显卡已经安装好了, 画面流畅解析度高; 画面停顿说明您可能选错了 DSDT 文件, 显卡未正确驱动.

至此, Mac OS 已经安装完毕, 下一篇将介绍几个必要驱动、设置.
