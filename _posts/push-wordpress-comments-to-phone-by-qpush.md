---
title: 妙用 QPush 向手机推送 WordPress 评论
tags:
  - QPush
  - WordPress
  - 手机
id: 473
categories:
  - 作品
date: 2014-02-13 09:30:15
---

[QPush 快推](http://qpush.me/)是一个新兴的从电脑向手机推送文字的工具, 此工具的作者是台湾人. 中国大陆软件 QQ 其实也包含推送功能, 而且能够双向推送, 推送的内容也不限文字, 无论图片、普通文件都行. 但 QPush 最大的特色是推送接口非常易用, 使用者不见得一定要用官方提供的推送工具, 只要有手机端的 Push name 和 Push code, 就能自己编写程序推送到手机了.

[![妙用 QPush 向手机推送 WordPress 评论](//img.beamnote.com/2014/push-wordpress-comments-to-phone-by-qpush.png)](//img.beamnote.com/2014/push-wordpress-comments-to-phone-by-qpush.png)<!-- more -->

有关 QPush 的介绍, 可以看看電腦王阿達写的[《QPush 快推 – 把各種訊息推送到手機的最簡單方案》](http://www.kocpc.com.tw/archives/4408), 相信事无巨细的描述让你比开发者还要会用这款应用. 不过, QPush 现在还只是 iOS 限定, 不知道开发者有无 Android 版本开发计划.

能够自由向手机推送消息是我一直期望的功能, 毕竟我们原本只能靠安装一款软件来接收有关此软件的通知, 不能够按照自己的需求发送. 可能有这些情境:

* 我使用非新浪微博官方客户端 WeicoPro, 我希望能收到微博评论的推送;
* 推送来自那些不支持推送功能邮箱的邮件通知;
* 每天抓取一些感兴趣的新闻推送到手机阅读;
* 监视我在某个网站上发布信息的回复;
* 明天天气不好的时候向我报告;
* ……

只要有相关服务开通了 API, 我们就能利用 API 获取信息并推送至手机, 个人感觉这是个很有意思的想法 :-).

我对 WordPress 做了一点儿试验, 加上了将评论通过 QPush 推送到手机的功能. 虽然可以通过邮件通知获得评论通知, 但 Gmail 的推送速度总是慢半拍, 而且它已经不推送被归在「动态」类别的邮件了.

例如, 我在博客留下一则评论:

[![在博客留下的评论](//img.beamnote.com/2014/qpush-1.png)](//img.beamnote.com/2014/qpush-1.png)

此评论在提交时被立刻推送到手机:

[![评论被推送至手机](//img.beamnote.com/2014/qpush-2.png)](//img.beamnote.com/2014/qpush-2.png)

打开即可看到全文:

[![查看推送全文](//img.beamnote.com/2014/qpush-3.png)](//img.beamnote.com/2014/qpush-3.png)

美中不足的是, 推送的链接不能直接点击.

在你主题的 `functions.php` 文件中加入以下内容, 也可以试一试 QPush 推送的神奇:

{% codeblock lang:php %}
function beamnote_qpush($comment_ID, $comment_approved)
{
  $push_name = '推名';
  $push_code = '推码';

  if ('spam' != $comment_approved &amp;&amp; $comment_approved)
  {
    $comment = get_comment($comment_ID);
    if (empty($comment))
    {
      return;
    }
    $post = get_post($comment->comment_post_ID);
    if ($comment->user_id == $post->post_author)
    {
      return;
    }
    if ('pingback' == $comment->comment_type || 'trackback' == $comment->comment_type)
    {
      return;
    }

    $message = "博客收到了来自 " . $comment->comment_author . " 的评论: \r\n";
    $message .= $comment->comment_content . "\r\n";
    $message .= "\r\n";
    $message .= "固定链接: " . get_permalink($comment->comment_post_ID). '#comment-' . $comment_ID;

    $params = array(
      'name' => $push_name,
      'code' => $push_code,
      'msg[text]' => $message
    );
    beamnote_async_post($params, 'qpush.me', '/pusher/push_site/');
  }
}

add_action('comment_post', 'beamnote_qpush', 10, 2);

// 别被它的名字骗了, 其实只是不等待对方返回的伪异步而已, 每个函数都是同步的.
function beamnote_async_post($params, $host, $path, $port = 80)
{
  $post_params = array();
  foreach ($params as $key => $val)
  {
    $post_params[] = $key . '=' . urlencode($val);
  }
  $post_body = implode('&amp;', $post_params);
  $content_length = strlen($post_body);

  $http_request = "POST $path HTTP/1.1\r\n";
  $http_request .= "Host: $host\r\n";
  $http_request .= "Content-Type: application/x-www-form-urlencoded; charset=UTF-8\r\n";
  $http_request .= "Content-Length: {$content_length}\r\n";
  $http_request .= "User-Agent: WordPress Comment Notifier\r\n";
  $http_request .= "\r\n";
  $http_request .= $post_body;

  if(false != ($fs = @fsockopen($host, $port, $errno, $errstr, 10)))
  {
    fwrite($fs, $http_request);
    fclose($fs);
  }
}
{% endcodeblock %}

为了避免产生骚扰, 待审核与垃圾评论都不会被推送, 在登录状态下产生的评论亦不被推送; 另外, 多人博客需要再做修改, 否则会接收到所有作者文章的评论.

QPush 属于刚刚起步的应用, 并没有开放 API 相关的宣告. 在官方没有说明的情况下, 现行的调用方式在未来也许会变动; 但如果能够保持开放的心态同时增强手机端功能, 相信能够受到更多人的欢迎.
