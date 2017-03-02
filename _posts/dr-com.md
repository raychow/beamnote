---
title: Dr.COM 小贴士
tags:
  - 下载
  - 网络
id: 90
categories:
  - 技巧
date: 2010-02-21 19:05:24
---

> 本文中 Dr.COM 均指代「Dr.COM Client 宽带上网认证客户端」.
Dr.COM 是城市热点公司的一款上网认证软件, 很多高校采用这一系统控制学生的网络接入. Dr.COM 实现了 IP 地址、MAC 地址与上网账号的绑定, 并通过防代理接入的杀手锏, 有效地阻止了学生私自共享网络的行为. 由于 Dr.COM本身技术不成熟, 导致了一些无法上网的问题, 为了使后来的同学们不再步前尘, 在这里给出使用 Dr.COM 的小贴士. <!-- more -->
> 2010.10.19 更新:
>
>
> 有时间折腾的同学可以尝试在沙盒中安装与运行 Dr.COM
>
>
> 请参见: [《用卡巴斯基掐死 Dr.COM》](//beamnote.com/2010/kaspersky-2011-throttle-dr-com/).
>
>
> 如果您已经安装 Dr.COM, 请在这之前卸载软件并重启电脑.

## 一、能以 Web 方式登录的, 勿用 Dr.COM 软件登录

Dr.COM 软件登录能带给你各种莫名的问题. 使用 Web 方式登录的同学们也无需阅读此文.

## 二、操作系统、网卡型号与 Dr.COM 版本选择

使用 Windows XP、Windows 2003 及更低版本操作系统, 建议选择一个能够登录的最低版本 Dr.COM, 因为新版本的 Dr.COM 防代理的技术肯定更强. 这里提供 Dr.COM 3.25 版本下载: [Box](http://www.box.net/shared/dqtbtkvk59), [Rayfile](http://www.rayfile.com/files/b1415ed1-1ed7-11df-9a6f-0015c55db73d/), [SkyDrive](http://cid-a848e4e0887932bc.skydrive.live.com/self.aspx/.Public/Dr.COM%5E_Clientv3.25.exe);

使用 Windows Vista、Windows 2008 及更高版本操作系统, 建议选择 Dr.COM 3.73 版本, 此版本可以兼容上述操作系统且不会安装 ARP 防火墙协议, 避免了与部分 Broadcom 网卡冲突导致死机 (联想 Y430、Y450 等型号笔记本电脑采用此系列网卡).
> Dr.COM 3.73 版本下载: [Box](http://www.box.com/shared/58vjmyzqb0), [Rayfile](http://www.rayfile.com/files/b15f7df5-1ed7-11df-83b0-0015c55db73d/), [SkyDrive](http://cid-a848e4e0887932bc.skydrive.live.com/self.aspx/.Public/Dr.COM%20Client%20%E6%A0%87%E5%87%86%E7%89%88-Ver3.73[%E5%B0%81%E8%A3%85][FOR%20WIN7%20VISTA%E8%87%AA%E5%8A%A8%E5%AF%BB%E6%89%BE%E6%8E%A5%E5%85%A5%E6%9C%8D%E5%8A%A1%E5%99%A8].rar);
>
>
> Dr.COM 5.2 版本下载: [Box](http://www.box.com/s/d05bf53e6378d65a3c63). 我没有用过此版本, 酌情使用;
>
>
> Dr.COM 全集下载: [Box](http://www.box.com/shared/npxjakm1xj), [SkyDrive](http://cid-a848e4e0887932bc.skydrive.live.com/self.aspx/.Public/Dr.com.rar).

## 三、运行 Dr.COM 时提示缺少 dll 文件

这一般是 WinPcap 没有安装成功所致. 在 [http://www.winpcap.org/install/default.htm]( http://www.winpcap.org/install/default.htm ) 下载最新版本的 WinPcap 并安装.

WinPcap 是网络封包抓取的一个工具, Dr.COM 靠它实现监控行为.

## 四、Windows 自动更新 80072F78 错误

在 Windows Vista、Windows 2008 及更高版本操作系统中使用除 Dr.COM 3.485、Dr.COM 3.71、Dr.COM 3.73 外的其它版本时会出现此问题; 此时国外品牌杀毒软件很可能也无法更新.

建议更换 3.73 版本解决此问题.

## 五、提示「驱动安装完毕, 需要重启本机」

一般只需要重新运行 Dr.COM, 如果仍然提示此消息, 请以管理员身份运行.

## 六、登录成功但无法访问互联网

这可能是由于 Dr.COM 破坏了 WinSock, 也可能是由于防火墙阻止了浏览器访问网络 (表现为 DNS 正常时能上 QQ 但是不能上网).

请按下列顺序进行操作, 直到问题解决:

1. 重新启动计算机;
2. 关闭病毒防护软件或防火墙, 如果此时可以访问外网, 对需要连接互联网的程序设置「例外」、「排除监视」或类似设定; 更换 Dr.COM 3.485 或 Dr.COM 3.71 版本也可能解决此问题 (配置 Broadcom 网卡的电脑谨慎使用);
3. 以管理员身份运行命令行提示符 (cmd), 输入 netsh winsock reset, 重置 winsock. 命令完成后, 重新启动计算机.

## 七、不再使用 Dr.COM

如果您不再需要使用 Dr.COM, 除了删除 Dr.COM 之外, 还需要进行以下步骤, 否则很可能无法正常访问互联网:

1. 删除 WinPcap;
2. 以管理员身份运行命令行提示符 (cmd), 进入 Dr.COM 的目录, 键入 TcpIPDogIns.exe -remove 并执行;
3. 继续在命令行提示符中输入 netsh winsock reset 并执行, 重置 winsock. 命令完成后, 重新启动计算机;
4. 删除 %windir%\System32\TcpIPDog*.dll.

## 八、登录时提示 Code(21)

如果确信学校没有禁止您当前版本的 Dr.COM, 请尝试:

1. 执行上一节中的后三个操作;
2. 关闭病毒防护软件或防火墙;
3. 以管理员身份运行 Dr.COM.
尝试成功后, 硬盘上应该存在 %windir%\System32\TcpIPDog*.dll.

## 九、登录时提示「登录超时失败」

1. 确认网线没有问题、网卡驱动正常;
2. 某些系统的无线网络优先权高于有线网络, 如果您开启了无线适配器, 请关闭.

## 十、使用 Internet Explorer 时需要刷新才能正常显示

这是因为 Dr.COM 与 IE 存在兼容性问题, 建议换用 Chrome、Firefox、Safari 等其它非 IE 内核浏览器, 速度更快.

* 请参阅第十三点.

## 十一、连接 VPN 时出现 619 错误

有三种方法, 不能保证一定成功:

* 在虚拟机中, 以 NAT 方式连接网络, 运行 Dr.COM;
* 使用 Open Dr.COM;
* 使用 L2TP 方式连接 VPN, 参见[此处](http://bbs.17173.com/topics/2703/200811/17/2582771,1.html).

## 十二、MySQL 不工作

此问题使用谷歌搜索可以得到解答, 但个人建议在虚拟机中调试 MySQL 或登录 Dr.COM, 总之 Dr.COM 与 MySQL 不能运行于同一环境.

* 有网友表示使用旧版本 (如 3.48) 可以与 MySQL 兼容, 但旧版本与 Windows Update、国外杀毒软件更新有冲突.

暂时的解决方法:

1. 有安装 Dr.COM 的, 先卸载 Dr.COM, 并执行第七节的操作;
2. 进入注册表编辑器, 导出以下两处键值, 并合并到同一个文件 (如 mysql.reg) :
> HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\WinSock2\Parameters\
>
>
> HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WinSock2\Parameters\

3. 安装 Dr.COM, 之后再次导出以上两处键值, 并合并到同一个文件 (如 drcom.reg);
4. 启动 MySQL 之前, 导入 mysql.reg;
5. 运行 Dr.COM 之前, 导入 drcom.reg.

## 十三、不使用 Dr.COM 接入网络时出现 400 Bad Request 错误

依旧是因为 Dr.COM 篡改了 Winsock, 并修改了发出的数据包, 导致服务器认为数据包错误.

以管理员身份运行命令行提示符 (cmd), 输入 netsh winsock reset, 重置 winsock. 命令完成后, 重新启动计算机.

## 十四、使用 Linux 与 Mac OS

现行的有三种方法:

* 使用虚拟机, 在虚拟的 Windows 中使用 Dr.COM, 此方法比较耗费资源;
* 使用 Wine (Linux) 或 CrossOver (Mac OS) 模拟 Windows 环境运行 Dr.COM;
* 使用第三方原生 Dr.COM, 请移步 [http://sourceforge.net/projects/drcom-client/files/](http://sourceforge.net/projects/drcom-client/files/), 这里包括了 Dr.COM for Windows / Linux / Mac OS, 并且支持代理.

## 如果有其它问题, 可以在此留言, 我会尽量帮助您.
