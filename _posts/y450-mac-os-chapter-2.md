---
title: Y450 拥抱黑苹果 (二)
tags:
  - Mac OS
  - 电脑
id: 178
categories:
  - 分享
date: 2010-05-22 21:10:47
---

在[《Y450 拥抱黑苹果 (一) 》](http://raychow.info/2010/y450-mac-os-chapter-1.html)中, 我们完成了 Mac OS 的安装. 值得一提的是, 尽管这是针对联想 Y450 设计的安装方案, 大部分步骤仍具有一般性, 只有 DSDT 与驱动部分不能套用.

在这一篇中, 我将列出我使用的所有驱动. 由于 Y450 有多种型号、配置, Mac OS 也不是为 PC 机设计的系统, 就算测试正常的驱动, 在您的计算机上也可能不工作. 目前我使用的驱动有:

[![Mac OS kext](http://img.beamnote.com/2010/mac-os-kext.png)](http://img.beamnote.com/2010/mac-os-kext.png)<!-- more -->

已经打包上传: [SkyDrive](http://cid-2343b4049d516106.skydrive.live.com/self.aspx/Ray%5E4s%20files/Mac%5E_OS%5E_X%5E_10%5E_6%5E_3%5E_Drivers%5E_For%5E_Y450.zip).

直接以压缩包的文件结构解压至 C:\Darwin\ 目录下即可.

使用驱动的原因是 DSDT 文件不能帮助 Mac OS 识别所有的硬件, 需要进行额外的补充.

压缩包的根目录下有两个文件, 放置于 C:\Darwin\ 目录:

1. com.apple.Boot.plist:
引导配置文件, 让 Mac OS 运行于 32 位, 不用每次开机都按 F8 手工输入;
2. smbios.plist:
修正内存显示, 非 DDR3 1066 的内存需要手动修改, 仅仅为了好看, 貌似没有实际作用;
在压缩包 \System\LibrarySL\Extensions\ 下是驱动文件, 既可以放置于 C:\Darwin\System\LibrarySL\Extensions\, 也可以放置在 Mac OS 盘\System\Library\Extensions\.

1. fakesmc.kext:
让 Mac OS 认为运行于苹果机;
2. CPUInjector.kext:
修正 CPU 显示;
3. OpenHaltRestart.kext:
解决关机与重启不正常的问题 (即黑屏后电脑无反应);
4. VoodooHDA.kext:
声卡驱动;
5. VoodooHDA.prefPane:
声卡控制台, 安装之后可以在「系统偏好设置」里找到「VoodooHDA」一项, 调节 Microphone 的数值可以使麦克风有效, 否则麦克风一直没有声音输入. 也可以使用此控制板调节音质;
[![VoodooHDA](http://img.beamnote.com/2010/voodoohda.png)](http://img.beamnote.com/2010/voodoohda.png)
6. AppleACPIPS2Nub.kext、VoodooPS2Controller.kext:
PS2 接口驱动, 用于驱动触摸板. 加入这两个文件后, 触摸板可以点击, 并开启了多点触摸功能, 在 iTunes、浏览网页时体验很好.
以使用 Chrome 浏览网页为例, 使用两根手指在触摸板上滑过, 网页跟随手指拖动; 使用两根手指点击, 打开右键菜单. 此驱动不支持缩放;
7. VoodooPS2.prefPane:
触摸板控制台, 安装之后可以在「系统偏好设置」里找到「VoodooPS2」一项, 比系统自带的触摸板设置功能强. 增加了「Cicular Scrolling」 (圆环滚动), 可以通过画圆的方式不停滚动.
[![VoodooPS2](http://img.beamnote.com/2010/voodoops2.png)](http://img.beamnote.com/2010/voodoops2.png)
8. AppleBCM5787MEthernet.kext:
网卡驱动, 安装此驱动稍微麻烦, 请按下列步骤进行:

    1. 在 Windows 中下载 [WinHex](http://www.greendown.cn/soft/8797.html) 并用其打开 AppleBCM5787MEthernet.kext\Contents\MacOS\AppleBCM5787MEthernet;
    2. 定位到 0002E910, 修改您的 MAC 地址:
       需要修改的地方以粗体标出, 下列示例对应的 MAC 地址是 BA:AD:FO:OD:BA:AD.
       ```
       0002E900 09 8B 45 0C C7 00 00 00 00 00 31 C0 C9 C3 55 89
       0002E910 E5 8B 55 08 8B 4D 0C B8 **BA** 00 00 00 90 90 88 01  ← 修改 1 项
       0002E920 B8 **AD** 00 00 00 90 90 88 41 01 B8 **FO** 00 00 00 90  ← 修改 2 项
       0002E930 90 88 41 02 B8 **OD** 00 00 00 90 90 88 41 03 B8 **BA** ← 修改 2 项
       0002E940 00 00 00 90 90 88 41 04 B8 **AD** 00 00 00 90 90 88  ← 修改 1 项
       0002E950 41 05 31 C0 C9 C3 90 90 90 90 90 90 90 90 55 89
       ```
    3. 保存这个文件, 将 AppleBCM5787MEthernet.kext 放置于正确位置;
    4. 进入 Mac OS 之后, 系统应该可以检测到网卡. 此时应该还不能上网, 每次开机都需要在终端执行下列指令:
sudo nohup tcpdump -i en0 > /dev/null
en0 如果不行, 尝试用 en1 代替.
为了避免麻烦, 可以编写一个可执行的文件放于桌面, 每次双击即可运行.
运行后, 可手动关闭终端窗口.
目前没有搞定的硬件还有无线网卡和电池, 如果有哪位搞定的麻烦知会一声.

还有一些关于硬件的补充:

1. 系统运行时无法调整亮度, 睡眠唤醒后亮度会变最高:
这属于 DSDT 文件不完善的问题. 要调整亮度, 需要在进入系统风火轮转动时进行;
2. 没有插网线时关机、重启不正常:
这是因为网卡驱动与 DSDT 文件不完善;
3. 电池电量无法显示:
这属于 DSDT 文件的问题;
4. 如果蓝牙无法开启:
请先到 Windows 中通过 Fn + F5 开启蓝牙;
5. 找不到  ⌘ (Command 键) 、![](http://km.support.apple.com/library/APPLE/APPLECARE_ALLGEOS/HT1343/ks_option.gif) (Option 键) :
请使用外接键盘, Command 键对应 Windows 键, Option 键对应于 Alt 键.
Y450 与黑苹果的故事先说到这里, 如果需要软件, 可以去 [MacIdea](http://www.macidea.com/thread-6689-1-1.html) 一探究竟.
