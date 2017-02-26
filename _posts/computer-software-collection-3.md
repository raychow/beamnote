---
title: 与时俱进的电脑软件大搜集 (三)
tags:
  - 电脑
  - 编辑器
  - 软件
id: 432
categories:
  - 技巧
date: 2012-12-12 22:08:34
---

前两期介绍的都是一些关乎安全性的软件, 好似要实现电脑的安全性需要多繁琐的步骤; 其实那几款软件都属于比较没有存在感的类型, 只需花很少的时间学习, 就能获得金钟罩铁布衫一般的防护, 怎么算都好过工作时出现错误带来损失的窘境.

离开安全软件的范畴, 我再推荐一些免费又好用的工具软件. 当然, 照样不是「新奇异缺」.

[![与时俱进的电脑软件大搜集 (三) ](http://img.beamnote.com/2012/computer-software-collection-3.jpg)](http://img.beamnote.com/2012/computer-software-collection-3.jpg)<!-- more -->

## 压缩 / 解压缩软件: 7-Zip

不知从何时起, WinRAR 就坐上了中国压缩软件安装量第一的宝座, 那时有盗版 Windows XP 的电脑, 就几乎装有盗版的 WinRAR. 有一些「小白」在网上下载了没有破解的原版进行安装, 对这款软件 40 天之后就不能使用的问题感到苦恼. 好在后来杀出两款免费压缩软件: 7-zip 与 2345 好压, 再也不用忍受每次打开 WinRAR 时的试用弹窗了 (幸亏 RAR 解码器是开源的, 否则那么多 RAR 压缩文件该怎么办\!).

现在就压缩表现对 7-zip、2345 好压与 WinRAR 做一个比较, 使用它们压缩一个 Visual C++ 源码文件夹. 用过比较新的 Visual Studio 的朋友都知道, 为了节省时间, Visual Studio 会将源码中用到的头文件与 Intellisense 等信息全部索引到数据库中, 致使一个小项目也会占上几十兆字节的空间.

[![待压缩的文件夹](http://img.beamnote.com/2012/folder_to_be_compressed.png)](http://img.beamnote.com/2012/folder_to_be_compressed.png)

测试文件夹大小为 360 MB, 测试结果如下:
<table>
<tbody>
<tr>
<td style="text-align: center;">格式</td>
<td style="text-align: center;">压缩软件</td>
<td style="text-align: center;">花费时间</td>
<td style="text-align: center;">大小</td>
<td style="text-align: center;">速度</td>
<td style="text-align: center;">压缩比</td>
</tr>
<tr>
<td rowspan="2">7z</td>
<td>7-Zip</td>
<td style="text-align: right;">2 分 34 秒</td>
<td style="text-align: right;">58,460 KB</td>
<td style="text-align: right;">2.34 MB/s</td>
<td style="text-align: right;">15.86%</td>
</tr>
<tr>
<td>2345 好压</td>
<td style="text-align: right;">5 分</td>
<td style="text-align: right;">58,465 KB</td>
<td style="text-align: right;">1.20 MB/s</td>
<td style="text-align: right;">15.86%</td>
</tr>
<tr>
<td rowspan="3">zip</td>
<td>7-Zip</td>
<td style="text-align: right;">56 秒</td>
<td style="text-align: right;">93,533 KB</td>
<td style="text-align: right;">6.43 MB/s</td>
<td style="text-align: right;">25.37%</td>
</tr>
<tr>
<td>2345 好压</td>
<td style="text-align: right;">31 秒</td>
<td style="text-align: right;">97,552 KB</td>
<td style="text-align: right;">11.61 MB/s</td>
<td style="text-align: right;">26.46%</td>
</tr>
<tr>
<td>WinRAR</td>
<td style="text-align: right;">15 秒</td>
<td style="text-align: right;">96,888 KB</td>
<td style="text-align: right;">24.00 MB/s</td>
<td style="text-align: right;">26.28%</td>
</tr>
<tr>
<td>rar</td>
<td>WinRAR</td>
<td style="text-align: right;">37 秒</td>
<td style="text-align: right;">75,004 KB</td>
<td style="text-align: right;">9.73 MB/s</td>
<td style="text-align: right;">20.35%</td>
</tr>
</tbody>
</table>

测试结果显示, 7z 格式有着最高的压缩比, 但相应所花费的时间也越多. 如果选择 7z 格式, 则 7-zip 显然比 2345 好压更快更优秀 (7z 压缩格式是作者自主开发的, 当然表现更好). 对于 zip 格式, WinRAR 居然拔得头筹, 不仅速度够快而且压缩文件的体积与其它软件相比相差不大.

当然, 严谨的测试需要更大的样本, 不同的压缩算法与参数选择显然会因不同样本有不同表现, 限于精力我就不继续测试了 ;-).

这里将 2345 好压与 7-zip 做一些其它方面的比较:

* 2345 好压的图标与界面更好看, 7-zip 的图标与界面只能用简陋来形容;
* 2345 好压的菜单设置更加合理 (尽管很少会用到 8-O);
* 2345 好压安装包附带了其它软件 (2345 看图王);
* 2345 好压在安装时要求设置浏览器首页;
* 2345 好压附带在线升级程序, 经常访问网络, 带来安全隐患;
* 两者均无广告.

2345 的一些缺点是国产软件的共性, 我更加推荐绿色、无网络访问的 7-zip.

## 纯文字编辑软件: Notepad++

记事本应该是很多人使用的第一款纯文字编辑器, 应急用倒还凑合, 但有些情况实在让人抓狂: 大文件会卡住、编码支持的有问题、还只支持微软自家的换行方式, 稍微有文字处理需求的使用者都应该换用其它软件.

无奈的是, 纯文字编辑软件虽然玲琅满目, 却几乎是一些收费产品. 以前用的是 UltraEdit, 似乎对中文支持的不够好, 而且它也不是免费的; 之后我就一直寻找合适的替代, 暂时找到最好 (且免费) 的也就是这一款编辑器, Notepad++.

[![Notepad++](http://img.beamnote.com/2012/notepad++.png)](http://img.beamnote.com/2012/notepad++.png)

不说那些各家都有的像编码格式转换、代码高亮、正则搜寻等的基本能力, Notepad++ 有一些功能做的很不错:

1\. 宏 (巨集)

Notepad++ 的宏简易又实用, 点一下工具栏上的按钮就能开始开始录制, 录制完成后能选择回放多次或者是运行到文件尾. 碰上什么重复性的工作, 例如加入行首空格, 点几下就能搞定.

但遗憾的是, 保存的宏似乎没有办法编辑, 也不能像 EmEditor (收费软件) 那样简简单单的编写脚本就能运行.

2\. 快捷键

Notepad++ 中几乎可以对所有功能设定快捷键, 无论是自有功能、宏、插件命令或是外部程序. 例如, 选中一段文字, 按下 `Alt + F3` 就能查询维基百科的解释.

3\. 插件支持

Notepad++ 的插件库很强大, 可以弥补一些没有实现的缺憾. 上图中的十六进制编辑模式, 便是 Hex-Editor 插件发挥效用的结果.

TextFx 是其附带的一个强大插件, 能够快速的实现匹配括号、格式化文本、增加行号、按行排序、清除空行等很多功能, 值得一试.
