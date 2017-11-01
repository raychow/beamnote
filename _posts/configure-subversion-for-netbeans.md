---
title: 超简单! 为 NetBeans 配置 SubVersion
tags:
  - NetBeans
  - SubVersion
  - 版本控制
id: 271
categories:
  - 技巧
date: 2010-12-02 18:36:56
---

话说本人之前除了 Visual Studio 之外, 一个 IDE 也没接触过……做 WP 开发时都是用 UltraEdit 等易上手白痴都会的轻量级编辑器的~

应该有不少程序员或者爱好者熟识 NetBeans 这款 Oracle 旗下的开源 IDE 吧, 我也抱着尝鲜的态度试用了下, 首先吓到我的是它超大的内存占用, 还有拖沓的速度 (原来习惯把程序做的卡卡的公司不止微软一家啊), 但代码自动完成、错误提示、代码重构功能很不错 (可惜重构在 WordPress 下不太好用), 最吸引我的是它与版本控制结合的很好, 默认就提供了对 CVS、Mercurial、SubVersion 的支持.

[![SubVersion](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/subversion.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/subversion.png)<!-- more -->

之前经常看见有些童鞋使用版本控制帮助自己码代码, 这可比以前我想起来就 WinRAR 打包一份强多了 (再用一次括号宣传下, 用盗版软件是不对滴, 推荐各位使用免费又强大的 [7zip](http://www.7-zip.org/), 善用佳软的介绍请[按此](http://xbeta.info/7-zip.htm)), 因为版本控制可以看到每个版本间的细微更改, 回溯任意文件, 对团队开发来说意义更大, 成员们可以放心的编写代码而不必过于担心同步、合并、冲突的事情.

还看过有童鞋使用 Dropbox 进行版本控制, 排除 Dropbox 就根本不存在的原因外, 我们的校园网十二点后可是要断网的, 这段时间怎么提交更新呢? 所以我决定使用易用的 SubVersion.

## 一、创建版本库与 NetBeans 设定

先在[这里](http://www.collab.net/downloads/subversion/)并安装找到适合操作系统的 SubVersion, 全名是 CollabNet Subversion Edge, 我居然点到了 Command-Line Client 上, 折腾了半个多小时也找不到建立版本库的功能.

前面说过, NetBeans 与版本控制配合得很好, 所以只要安装 SubVersion 就可以啦, 至于乌龟神马的完全不需要.

SubVersion 自带了 Apache, 所以我们可以通过它建立的捷径访问后台 (当然是 localhost 上咯, 端口不知道是不是都一样), 初始用户名与密码都是 admin, 幸好后台是中文的.

[![Collabnet Subversion Edge](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-edge.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-edge.png)

首先要做的, 是配置好 SubVersion 的版本库父文件夹. 按我的理解, 创建一个版本库可以看作是添加一个新项目, SubVersion 的多数操作都是针对版本库进行的.

选择「管理」 - 「服务器配置」, 在「版本库父文件夹」键入你要放置版本库的文件夹, 这个文件夹并不是你开发时使用的文件夹, 而是记录一个版本库的配置、修订版本和锁定状态的资料库, 版本库与项目文件夹就好比是档案馆与你的家的关系.

[![Collabnet Subversion Edit](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-edit.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-edit.png)

下一步是创建新版本库. 选择「版本库」 - 「新版本库」, 键入版本库的名字, 单击「创建」. 「创建标准的主干 / 分支 / 标签结构」根据你个人喜好勾选, 不过还是推荐创建啦, 这样版本会更加有序.

[![Create Subversion Library](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-create-library.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-create-library.png)

创建版本库后, 你就可以在刚才设置的版本库父文件夹中找到你的版本库咯.

[![View Subversion Folder](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-folder.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-folder.png)

以后就不要随意修改此文件夹中的内容了, 以免造成不可挽回的损失唷

从现在开始就可以在 NetBeans 中设定 SubVersion 了.

打开 NetBeans, 选择「工具」 - 「选项」 - 「其他」, 在左边选择「Subversion」, 键入 SVN 可执行文件的路径 (例如, C:\Program Files\csvn\bin), 单击「确定」保存.

## 二、导入版本库

下面我们就要在 NetBeans 中把资料导入版本库里啦\! 假设我们要进行版本控制的项目的目录位于 D:\test\, 按如下步骤进行:

1. 选择要进行版本控制的项目;
2. 选择「团队开发」 - 「Subversion」 - 「导入到资源库中...」, 打开导入窗口;
3. 输入资源库 URL, 直接从 SubVersion 后台的版本库列表「检出命令」复制就可, 例如 `http://RayChow-PC:81/svn/test`, 再键入用户名与密码, 单击「下一步」;
[![Import](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-import1.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-import1.png)
4. NetBeans 会尝试连接 SubVersion 库, 之后选择导入到 trunk 资源库, 并键入指定消息. 以后每次提交都输入消息是一个好习惯, 这样你就知道每次更新了什么;
[![Import](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-import2.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-import2.png)
5. 最后一步 NetBeans 询问你需要导入到文件, 可以看见他把自己创建的文件都列出来了. 一般都要全部导入啦;
6. 单击「完成」, 可以看见项目列表中被导入的文件变为绿色, 导入成功后再次恢复黑色.
访问刚才输入的资源库 URL 可以看见汇入的项目, 这些目录和文件已被记录在案, 任何时候你都能调出查看, 哪怕他们在项目中已被修改或删除.

[![View Repository](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-view-repository.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-view-repository.png)

至此, 版本库准备完毕.

## 三、签出工作拷贝

什么叫做签出工作拷贝呢? 按我的理解, 签出就是把版本库中项目的最新版本完整的下载到本地, 并与版本库建立联系, 这样才能完成「提交」工作.

由于第二点中导入操作已经使项目文件夹与版本库建立了联系, 因此第二点操作完成后不需要再进行签出.

在你更换项目文件夹、有其他人或其它电脑加入到你的项目中时, 就需要签出来获得最新版本了.

NetBeans 进行签出工作也很简单, 按如下步骤进行:

1. 选择要签出到的项目;
2. 选择「团队开发」 - 「Subversion」 - 「签出...」, 打开签出窗口;
3. 输入资源库 URL、用户名、密码, 与第二点中相同;
4. 「要签出的文件夹」这一步, 资源库文件夹选择你需要签出的文件夹, 例如 trunk;
资源库修订一般留空, 表示获取最新版本;
由于刚才选定了 trunk 文件夹, 所以勾选 跳过「trunk」且仅签出其内容, 可以看见「工作副本」发生变化;
5. 单击「完成」, 进行签出.

## 四、颜色

一旦项目中的文件作出修改, 项目窗口中的文件名会根据以下规则改变样式:

* 绿色: 新加入的文件;
* 蓝色: 被修改的文件;
* 红色: 有冲突的文件;
* 灰色: 被忽略的文件;
* 删除线: 提交时被排除的文件.

## 五、提交

提交是将本地项目做出的修改应用到版本库的过程.

按如下步骤进行:

1. 选择要提交的项目或文件;
2. 选择「团队开发」 - 「提交...」, 打开提交窗口;
[![Commit](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-commit.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-commit.png)
3. 输入提交消息, 下方的列表中可以排除或比较文件 (通过右键);
4. 单击「提交」完成操作.

## 六、更新

更新是检查版本库的文件自上次同步后有无改变的过程. 按我的理解, 更新会下载版本库中新建的文件、替换上次同步后本地未作出修改而版本库版本更新的文件、删除上次同步后本地仍存在而版本库中已被删除的文件, 并标示有冲突的文件.

如果是团队开发, 应该经常检查更新; 如果是个人开发, 更新一万次也没有作用……

## 七、冲突

冲突是指上次同步后本地对某文件作出修改, 提交时版本库中的版本比本地更新而导致冲突的情况. 例如, 我在 A 文件添加了函数 B, 但提交时发现同事 C 已经在 A 文件修改了 类 D 并作出了提交, 此时 A 文件已经不能继续提交了, 否则会覆盖同事 C 的修改.

在更新后如果发现冲突, NetBeans 会列出冲突的地方, 在右下方的版本输出控制窗口中右击文件选择「解决冲突」可以更直观的对比并解决冲突, 或者……干脆直接放弃你的修改.

[![Conflict](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-conflict.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-conflict.png)

## 八、显示更改 与 比较

这两个功能在我看来没有太大区别, 都是用来查阅上次更新后本地做出的修改的, 唯一的不同是: 「显示更改」只会列出被修改的文件, 双击文件可以查看详细对比, 而「比较」则直接显示列表与详细对比.

[![Status](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-status.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-status.png)

## 九、还原修改

假定在比较之后你发现对某个文件的所有修改都是错误的, 或许根本不应该修改这个文件, 或者是从开头重新修改会更加容易, 就是使用「还原修改」的时候了.

[![Revert](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-revert.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/svn-revert.png)

* 还原本地修改: 将文件还原到未修改的状态, 从文件所在文件夹的 .svn 目录中直接复制, 不经过服务器;
* 根据单个提交还原修改: 从版本库中获得指定的版本, 如果有冲突还需要解决;
* 根据早期的提交还原更改: 我还不清楚……似乎会把起始与结束的版本都拉回来, 再丢给你解决冲突.

## 十、基本的工作周期

日常使用时, 一般按照以下步骤进行版本控制:

1. 检出或更新你的工作拷贝;
2. 作出修改. 请注意, 对于工作拷贝的目录操作 (如增加、删除、移动、复制) 最好在 NetBeans 中进行, 或者通过 svn 命令直接操作, 如果直接使用不相关的文件管理工具操作, 可能会导致错误;
3. 检查更改 (即第六点);
4. 如果有必要, 还原修改;
5. 如果存在冲突, 则解决;
6. 提交更改.

以上操作均可以使用 NetBeans 方便的完成.
