---
title: WordPress 整合 Google Ajax 搜索
tags:
  - Google
  - WordPress
  - 博客
  - 搜索
  - 网络
id: 235
categories:
  - 技巧
date: 2010-08-10 22:51:25
---

Google 自定义搜索提供了可定制性极强的搜索功能, 可以与 WordPress 完美整合. 在看了自力博客[《WordPress 整合 Google 自定义搜索》](http://www.hzlzh.com/wordpress-google-site-search/)一文后, 我决定改造博客的搜索功能, 减轻服务器负担.

与此同时, 我参考了老肥博客的[《打造完美的 AJAX 版 Google 自定义搜索》](http://fis.io/ajax-google-custom-search-engine.html), 在试验站上敲敲打打了一段时间, 实现了如下功能:

* 非搜索页面纯 HTML 代码, 不加载不相关资源;
* 搜索页面整合 Google 自定义搜索;
* 搜索页面中, 搜索结果随文字的输入动态呈现, 无需单击「搜索」.

[![WordPress 整合 Google Ajax 搜索](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/wordpress-google-ajax-search.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/wordpress-google-ajax-search.png)<!-- more -->

## 一、申请 Google 自定义搜索

访问[此处](http://www.google.com/cse/)创建自定义搜索引擎, 要搜索的网站可填入 `www.example.com/*` (www.example.com 为您的网站);

如果只要搜索文章页, 固定链接的结尾需要有固定字符 (如 .html), 那么要搜索的网站可填入 `www.example.com/*.html`.

在「外观」选项中, 选择托管「搜索元素」、「两栏」布局并保存.

接下来, 还可以在控制面板中修改搜索引擎的细节. 尽管可以关掉广告, 但该选项仅适用于已注册的非营利组织、大学和政府机构, 建议不属于这些行列的博客, 在「赚钱」选项中将搜索引擎与 Google Adsense 帐户关联, 这样还能带来额外收入.

## 二、修改搜索框

不同的主题搜索框代码位置是不一样的, Mystique 搜索框放在单独的 searchform.php 中, 其它的主题请自行寻找.

将搜索框代码替换成自己的内容, 例如:

{% codeblock lang:html %}
<form action="/search" id="searchbox">
    <input type="text" name="q" id="search-text" />
    <input type="submit" value="搜索" />
</form>
{% endcodeblock %}

我是直接把 Google 的样式复制过来了, 但注意各元素的 Class 不要与 Google 自动加载的 default.css 有重复, 否则可能会变形的.

## 三、建立搜索结果页

### 1\. 新建页面

在这一步, 我认为将代码写入模板比直接写入页面要好, 写入模板可以自定义整个页面, 避免搜索页面出现评论模块的尴尬.

说件囧事, 开始我把新建页面理解成在网站根目录新建一个文件了, 还以为设定了 755 权限就可以执行, 脑残了……

先在当前 WordPress 主题的目录下新建一个模板文件, 文件名自定, 如 cse.php, 先在其中输入以下内容:

{% codeblock lang:php %}
<?php /* Template Name: CSE */ ?>
{% endcodeblock %}

接着在 WordPress 后台选择「添加新页面」, 写好标题, 将固定链接设定为 search (对应于搜索框代码中的 action), 并选择「CSE」模板.

保存后查看页面, 正确的话应该一片空白.

### 2\. 写入代码

把索引页模板 (index.php) 的内容插入到 cse.php 的结尾, 删掉正文内容.

进入 Google 自定义搜索的「获取代码」, 将下方文本框中的内容 (用于显示搜索结果) 插入 cse.php, 再将上方文本框中第二行之后的内容 (搜索引擎主程序) 插入:

{% codeblock lang:js %}
<div id="cse" style="width:100%;">载入中……</div>
<script src="http://www.google.com/jsapi" type="text/javascript"></script>
<script type="text/javascript">
    google.load('search', '1', {language : 'zh-CN'});
    google.setOnLoadCallback(function() {
        var customSearchControl = new google.search.CustomSearchControl('000000000000000000000:00000000000');
        customSearchControl.setResultSetSize(google.search.Search.FILTERED_CSE_RESULTSET);
        var options = new google.search.DrawOptions();
        options.setSearchFormRoot('cse-search-form');
        options.setAutoComplete(true);
        customSearchControl.draw('cse', options);
    }, true);
</script>
<link rel="stylesheet" href="http://www.google.com/cse/style/look/shiny.css" type="text/css" />
{% endcodeblock %}

`000000000000000000000:00000000000` 应替换为 Google 自定义搜索控制面板基本信息中「搜索引擎的唯一 ID」.

现在这段代码还不能正常工作, 它没有获得刚刚传来的搜索关键字, 也没有设定使用 AJAX 搜索, 需要做一点修改:

#### 1) 获得关键字

通过 PHP 直接获得表单传入的数据:

{% codeblock lang:php %}
var search = '<?php echo $_GET['q']; ?>';
{% endcodeblock %}

#### 2) 为 Google 指定搜索框

Google 提供了两种方式指定由 Google 使用的搜索框:

##### a. 替换搜索表单

{% codeblock lang:php %}
options.setSearchFormRoot('cse-search-form');
{% endcodeblock %}

`cse-search-from` 应替换为搜索表单 id, 或者任何指定的 DOM 容器.

此方法存在一个非常雷人的问题, 在 IE 中点击一次「搜索」按钮, 搜索框居然变样了……方法 B 则无此问题.

##### b. 传递输入元素

{% codeblock lang:js %}
options.setInput(document.getElementById('search-text')); // 传递输入元素
document.getElementById('searchbox').setAttribute('onSubmit',"document.getElementById('search-text').select(); return false;"); // 按下搜索按钮选中搜索框, 并阻止表单提交
document.getElementById('search-text').value = search; // 设置搜索框的内容
{% endcodeblock %}

其中 `search-text` 应替换为搜索文本框 id, `searchbox` 应替换为搜索表单 id.

该方法可以实现随文字输入动态改变搜索结果, 此时搜索按钮实际上没有任何用处.

不过, 如果打开了自动完成, 搜索时按下回车会导致发生两次查询——尽管不用按回车, 页面被刷新两次也很烦人, 只要删除打开自动完成的代码即可:

<del datetime="2011-01-13T10:54:38+00:00">options.setAutoComplete(true);</del>

#### 3) 执行搜索

{% codeblock lang:js %}
customSearchControl.execute(search);
{% endcodeblock %}

#### 最后完工的代码

{% codeblock lang:html %}
<div id="cse">载入中……</div>
<script src="http://www.google.com/jsapi" type="text/javascript"></script>
<script type="text/javascript">
    google.load('search', '1', {language : 'zh-CN'});
    google.setOnLoadCallback(function() {
        var customSearchControl = new google.search.CustomSearchControl('000000000000000000000:00000000000');
        customSearchControl.setResultSetSize(google.search.Search.FILTERED_CSE_RESULTSET);
        customSearchControl.setLinkTarget(google.search.Search.LINK_TARGET_SELF);
        var options = new google.search.DrawOptions();
        var search = '<?php echo $_GET['q'] ; ?>';
        // options.setSearchFormRoot('cse-search-form'); // Google 搜索框
        options.setInput(document.getElementById('search-text')); // 自定义搜索框
        document.getElementById('searchbox').setAttribute('onSubmit',"document.getElementById('search-text').select(); return false;"); // 自定义搜索框
        document.getElementById('search-text').value = search; // 自定义搜索框
        customSearchControl.draw('cse', options);
        customSearchControl.execute(search);
    }, true);
</script>
{% endcodeblock %}

具体类与函数请参考 [Google AJAX 搜索 API 开发人员指南](http://code.google.com/intl/zh-CN/apis/ajaxsearch/documentation/), 其中还有不少有趣的示例.

最后, 就剩下美化工作啦, 我就偷懒直接使用自带样式, 也挺搭的~

### 番外: 超简单侧边栏 AJAX 搜索

本来我用的这方法的, 但有个弊端就是每个页面都要加载与搜索有关的 JS.

在「外观」选项中, 选择「压缩视图」布局，单, 「获取代码...」，将, 码直接放入「文本」小工具中，立, 实现 AJAX 无刷新的搜索.
