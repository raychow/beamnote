---
title: 使用 Cron jobs 自动备份网站 (一)
tags:
  - 博客
  - 备份
id: 211
categories:
  - 技巧
date: 2010-08-02 15:22:19
---

经常备份网站是一个良好的习惯, 但无论是 cPanel 面板或是 WordPress后台都没有提供自动备份的功能. 如果手动定期备份, 不仅操作麻烦, 还容易忘记; WordPress 虽然有备份插件, 但有些人对插件有恐惧症, 认为少用为好. 其实, 我们还可以利用 cPanel 面板中的 [Cron jobs](http://zh.wikipedia.org/zh-cn/Cron) 备份网站 (DirectAdmin 面板呢? 我无视……).

[![Cron jobs](http://img.beamnote.com/2010/cpanel-automatic-backup-chapter-1.png)](http://img.beamnote.com/2010/cpanel-automatic-backup-chapter-1.png)<!-- more -->

搜索到一篇[《cPanel自动备份教程之完全备份篇》](http://www.ifred.cn/cp-full-backup/), 利用 Cron jobs 定期执行给定的 php 文件进行备份, 实际上这个 php 文件起到了自动填写表单的作用, 与我们在后台点击「下载或生成全备份」操作是一样的, 但此文中给出的代码需要填写双份用户名与密码, 我看到密码明文存储到服务器中觉得就不踏实 (尽管 php 文件放在根目录下, 通过域名是无法访问的). 详读了代码, 发现这段程序将「备份目的地」选项指定到了远程 FTP 服务器, 难怪要求连 FTP 帐号密码都要输入了

我对这段代码稍加改动, 现在已经没有明文用户名密码了, 但是依然要提供用于登录 cPanel 面板的认证码.

{% codeblock lang:php %}
// 此PHP程序配合Cron jobs使用可以定期自动产生cPanel备份,
// 以max.hedroom在cpanel.net论坛的文章为程序基础,
// 这个程序包含密码, 请注意档案安全.
// 中文翻译修正: Fred, http://www.ifred.cn/cp-full-backup
// 程序修改: Ray Chow, http://raychow.info/2010/cpanel-automatic-backup.html
// ********* 下列项目需要设定 *********
// cPanel登入信息
$domain = "example.com"; // cPanel绑定的域名(或ip)
$skin = "x3"; // 设定正在使用的cPanel模板(例如:x, x3, rvblue)
$pass = ""; // 填入在 getpass.php 中获得的认证码
// 通知信息
$notifyemail = "exapmle@example.com"; // 寄发执行结果的e-mail位置
// 安全模式
$secure = 0; // 0为标准http, 1为ssl(需要主机支持ssl功能)
// 设定值为1时会在排程记录中产生网页报告
$debug = 0;
// *********** 以下不需更改 *********
if ( $secure ) {
    $url = "ssl://".$domain;
    $port = 2083;
} else {
    $url = $domain;
    $port = 2082;
}
$socket = fsockopen( $url, $port );
if ( !$socket ) {
    echo "Failed to open socket connection... Bailing out!\n";
    exit;
}
$params = "dest=homedir&email=$notifyemail&submit=Generate Backup";
// Make POST to cPanel
fputs( $socket, "POST /frontend/".$skin."/backup/dofullbackup.html?".$params." HTTP/1.0\r\n" );
fputs( $socket, "Host: $domain\r\n" );
fputs( $socket, "Authorization: Basic $pass\r\n" );
fputs( $socket, "Connection: Close\r\n" );
fputs( $socket, "\r\n" );
// Grab response even if we don't do anything with it.
while ( !feof( $socket ) ) {
$response = fgets( $socket, 4096 );
if ( $debug )
    echo $response;
}
fclose( $socket );
{% endcodeblock %}
> 按这里下载: [cp_full_backup.zip](http://raychow.info/wp-content/uploads/2010/08/cp_full_backup.zip)

根据自己网站的实际情况修改填入的信息.

其中, 正在使用的 cPanel 模板可以直接通过 cPanel 登入后的网址判断, http://example.com:2082/frontend/x3/index.html 这个网址中, x3 就是 cPanel 模板;

$pass 变量中需要的认证码由将登录 cPanel 的帐号与密码连接以 BASE64 编码得到, 使用这段小程序可以得到认证码.

{% codeblock lang:php %}
$cpuser = ""; // 登录 cPanel 的帐号
$cppass = ""; // 登录 cPanel 的密码
$authstr = $cpuser . ":" . $cppass;
$pass = base64_encode( $authstr );
echo $pass;
{% endcodeblock %}
> 按这里下载: [cp_get_pass.zip](http://raychow.info/wp-content/uploads/2010/08/cp_get_pass.zip)

输出的文字即为 cp_full_backup.php 中 $pass 变量的值, 获得认证码后立即删除 cp_get_pass.php 防止认证码泄漏.

或者, 您也可以在[这里](http://tool.chinaz.com/Tools/Base64.aspx)输入「帐号 + 分号 + 密码」 (不含引号), 例如 account:password, 进行 BASE64 加密, 也可以得到认证码.

下面, 将修改好的 cp_full_backup.php 放在根目录中 (即 /home/username, 而非 public_html、www 目录, 否则可以被其他人运行, 有极大风险), 进行 Cron jobs 的设定.

进入 Cron jobs, 设定备份间隔的时间, 在 Command 中填入 `/usr/bin/php /home/username/cp_full_backup.php` (将 username 换成登录您的用户名), 最后单击 Add New Cron Job.

[![Cron jobs](http://img.beamnote.com/2010/2010-08-02_14-48-23.png)](http://img.beamnote.com/2010/2010-08-02_14-48-23.png)

图中为一天备份两次, 实际不需要这么高频率. 需要注意的是, 备份时间为主机指定的时间, 所以一些在美国的主机晚间 12 点是中国的午间时刻.

为确保备份已成功设定, 您可以先新建一个每分钟执行的定时任务. 如果下一分钟收到了通知备份的邮件, 证明自动备份已经成功运作 (记得测试完成要删除啊, 否则撑爆你的主机~).

呼呼, 看来敲了不少字, 本来想再试试只备份数据库的, 篇幅限制, 放在下篇吧.
