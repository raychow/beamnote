---
title: WordPress 添加自定义栏目面板
tags:
  - WordPress
  - 博客
  - 自定义栏目
id: 267
categories:
  - 技巧
date: 2010-11-19 14:01:32
---

WordPress 的自定义栏目 (在 WordPress 2.9.x 中还称为「自定义域」, 以下统称「自定义栏目」) 功能为更方便的拓展 WordPress 功能提供了可能, 然而默认的自定义栏目面板就不太友好了, 很多自定义栏目都直接用英文储存, 需要填写的时候还要手动添加; 只能填写文本, 对于某些特殊要求 (例如布尔型及有限个枚举) 的自定义栏目还要记住填写方式.

[![WordPress 自定义字段](//img.beamnote.com/2010/custom-fields.png)](//img.beamnote.com/2010/custom-fields.png)<!-- more -->

我先推荐一个相当强大的插件: [Custom Field Template](http://wordpress.org/extend/plugins/custom-field-template/), 因为要自定义的东西比较多, 所以设置页面显得很繁杂, 但要修改的内容并不多.

对于主题开发者来说, 总是希望集成更多的功能, 也应该尽可能的帮助使用者方便使用主题, 如果告诉客户, 要添加产品信息需要先在「自定义栏目」里找到 product_info, 点击「添加」再输入值, 似乎是一件很囧的事情. 所幸这篇网志: [《Control your own WordPress custom fields》](http://sltaylor.co.uk/blog/control-your-own-wordpress-custom-fields/)解决了大部分的问题.

> 囧啊囧, 忘记放效果示例了, 大家别再问这是神马东西了, 下图就是, 它出现在写文章的文本框的下方……
>
> [![WordPress 自定义字段](//img.beamnote.com/2010/my-custom-field.png)](//img.beamnote.com/2010/my-custom-field.png)

按照惯例, 我们需要在主题的 `functions.php` 中插入代码, 当然你要根据自己的需要修改啦.

## 一、定义 myCustomFields 类

{% codeblock lang:php %}
if ( !class_exists('myCustomFields') ) {
    class myCustomFields {
        /**
        * @var  string  $prefix  自定义栏目名的前缀
        */
        var $prefix = '_mcf_';
        /**
        * @var  array  $postTypes  包含了标准的「文章」、「页面」和自定义文章类型的数组, 自定义栏目面板将出现在这些文章类型中
        */
        var $postTypes = array( "page", "post" );
{% endcodeblock %}

如果自定义栏目名是以下划线(`_`)开头的, 它将在 WordPress 自带的「自定义栏目」面板中隐藏. 如果想保留 WordPress 自带「自定义栏目」面板, 建议使用这个方法.

## 二、定义所有的自定义栏目

{% codeblock lang:php %}
        /**
        *  @var  array  $customFields  定义了所有的自定义栏目
        */
        var $customFields = array(
            array(
                "name"          => "block-of-text",
                "title"         => "多行文本",
                "description"   => "",
                "type"          => "textarea",
                "scope"         =>    array( "page" ),
                "capability"    => "edit_pages"
            ),
            array(
                "name"          => "short-text",
                "title"         => "单行文本",
                "description"   => "",
                "type"          =>    "text",
                "scope"         =>    array( "post" ),
                "capability"    => "edit_posts"
            ),
            array(
                "name"          => "checkbox",
                "title"         => "复选框",
                "description"   => "",
                "type"          => "checkbox",
                "scope"         =>    array( "post", "page" ),
                "capability"    => "manage_options"
            )
        );
{% endcodeblock %}

所有的自定义栏目都应该写在这里, 当然要遵循这样的格式, 不过这里只给出了单行文本框、多行文本框、富文本框、复选框四种类型, 您可以在稍后自由添加.

每一个字段包含的值为:

* `name`: 自定义栏目的名称. 这个名称将加上前缀存储在数据库中;
* `title` : 用于显示在自定义栏目面板的标题;
* `description` : 显示在每个自定义栏目的下方, 给出提示;
* `type` : 定义了显示的类型, 默认为单行文本 (`text`);
* `scope` : 定义了每个自定义栏目的作用范围, 例如 `post` 文章、`page` 页面;
* `capability` : 定义了编辑每个自定义栏目需要的权限, 如果对此不太了解, 参见这篇文章: [《Users, roles, and capabilities in WordPress》](http://justintadlock.com/archives/2009/08/30/users-roles-and-capabilities-in-wordpress).

## 三、用于初始化的相关函数

{% codeblock lang:php %}
        /**
        * 用于兼容 PHP 4 的构造函数
        */
        function myCustomFields() { $this->__construct(); }
        /**
        * 用于 PHP 5 的构造函数
        */
        function __construct() {
            add_action( 'admin_menu', array( &amp;$this, 'createCustomFields' ) );
            add_action( 'save_post', array( &amp;$this, 'saveCustomFields' ), 1, 2 );
            // 如果想要保留 WordPress 自带「自定义栏目」面板, 请注释下一行
            add_action( 'do_meta_boxes', array( &amp;$this, 'removeDefaultCustomFields' ), 10, 3 );
        }
        /**
        * 移除 WordPress 自带「自定义栏目」面板
        */
        function removeDefaultCustomFields( $type, $context, $post ) {
            foreach ( array( 'normal', 'advanced', 'side' ) as $context ) {
                foreach ( $this->postTypes as $postType ) {
                    remove_meta_box( 'postcustom', $postType, $context );
                }
            }
        }
        /**
        * 创建新的「自定义栏目」面板
        */
        function createCustomFields() {
            if ( function_exists( 'add_meta_box' ) ) {
                foreach ( $this->postTypes as $postType ) {
                    add_meta_box( 'my-custom-fields', 'Custom Fields', array( &amp;$this, 'displayCustomFields' ), $postType, 'normal', 'high' );
                }
            }
        }
{% endcodeblock %}

其中包括了构造函数 (注意 PHP 4 与 PHP 5 承认的构造函数并不相同, 而 PHP 4 似乎没有析构函数), 向 `admin_menu` 添加钩子以显示自定义栏目面板, 向 `save_post` 添加钩子以在文章保存时也能保存自定义栏目中的信息, 向 `do_meta_boxes` 添加钩子以移除 WordPress 自带「自定义栏目」面板 (如果不需要可以将之注释).

## 四、权限检查

{% codeblock lang:php %}
        /**
        * 显示新的「自定义栏目」面板
        */
        function displayCustomFields() {
            global $post;
            ?>
            <div class="form-wrap">
                <?php
                wp_nonce_field( 'my-custom-fields', 'my-custom-fields_wpnonce', false, true );
                foreach ( $this->customFields as $customField ) {
                    // 范围检查
                    $scope = $customField[ 'scope' ];
                    $output = false;
                    foreach ( $scope as $scopeItem ) {
                        // 原作者在此处使用了 switch, 估计是修改时忘记删掉了
                        if ( $post->post_type == $scopeItem ) {
                            $output = true;
                            break;
                        }
                    }
                    // 权限检查
                    if ( !current_user_can( $customField['capability'], $post->ID ) )
                        $output = false;
{% endcodeblock %}

这一段是显示新的「自定义面板」的函数 `displayCustomFields()`, [`wp_nonce_field()`](http://codex.wordpress.org/Function_Reference/wp_nonce_field) 函数生成了一个包含随机数的隐藏字段, 用于检查提交的内容来自合法的位置.

首先检查的是自定义栏目的作用范围，如, 释中所述，原, 者在此处使用了 `switch`，估, 是修改时忘记删掉了 (因为前一个版本检查方式与现在有一些差别). 接着检查的是权限，检, 通过后才会显示这个自定义栏目.

## 五、输出自定义栏目

{% codeblock lang:php %}
                    // 如果允许, 则输出
                    if ( $output ) { ?>
                        <div class="form-field form-required">
                            <?php
                            switch ( $customField[ 'type' ] ) {
                                case "checkbox": {
                                    // 输出复选框
                                    echo '<label for="' . $this->prefix . $customField[ 'name' ] .'" style="display:inline;"><b>' . $customField[ 'title' ] . '</b></label>&nbsp;&nbsp;';
                                    echo '<input type="checkbox" name="' . $this->prefix . $customField['name'] . '" id="' . $this->prefix . $customField['name'] . '" value="yes"';
                                    if ( get_post_meta( $post->ID, $this->prefix . $customField['name'], true ) == "yes" )
                                        echo ' checked="checked"';
                                    echo '" style="width: auto;" />';
                                    break;
                                }
                                case "textarea":
                                case "wysiwyg": {
                                    // 输出多行文本框
                                    echo '<label for="' . $this->prefix . $customField[ 'name' ] .'"><b>' . $customField[ 'title' ] . '</b></label>';
                                    echo '<textarea name="' . $this->prefix . $customField[ 'name' ] . '" id="' . $this->prefix . $customField[ 'name' ] . '" columns="30" rows="3">' . htmlspecialchars( get_post_meta( $post->ID, $this->prefix . $customField[ 'name' ], true ) ). '</textarea>';
                                    // 输出富文本编辑框
                                    if ( $customField[ 'type' ] == "wysiwyg" ) { ?>
                                        <script type="text/javascript">
                                            jQuery( document ).ready( function() {
                                                jQuery( "<?php echo $this->prefix . $customField[ 'name' ]; ?>" ).addClass( "mceEditor" );
                                                if ( typeof( tinyMCE ) == "object" && typeof( tinyMCE.execCommand ) == "function" ) {
                                                    tinyMCE.execCommand( "mceAddControl", false, "<?php echo $this->prefix . $customField[ 'name' ]; ?>" );
                                                }
                                            });
                                        </script>
                                    <?php }
                                    break;
                                }
                                default: {
                                    // 输出单行文本框
                                    echo '<label for="' . $this->prefix . $customField[ 'name' ] .'"><b>' . $customField[ 'title' ] . '</b></label>';
                                    echo '<input type="text" name="' . $this->prefix . $customField[ 'name' ] . '" id="' . $this->prefix . $customField[ 'name' ] . '" value="' . htmlspecialchars( get_post_meta( $post->ID, $this->prefix . $customField[ 'name' ], true ) ). '" />';
                                    break;
                                }
                            }
                            ?>
                            <?php if ( $customField[ 'description' ] ) echo '<p>' . $customField[ 'description' ] . '</p>'; ?>
                        </div>
                    <?php
                    }
                } ?>
            </div>
            <?php
{% endcodeblock %}

如果要添加新的显示类型, 就是这里了.

## 六、保存自定义栏目的值

{% codeblock lang:php %}
        /**
        * 保存自定义栏目的值
        */
        function saveCustomFields( $post_id, $post ) {
            if ( !wp_verify_nonce( $_POST[ 'my-custom-fields_wpnonce' ], 'my-custom-fields' ) )
                return;
            if ( !current_user_can( 'edit_post', $post_id ) )
                return;
            if ( ! in_array( $post->post_type, $this->postTypes ) )
                return;
            foreach ( $this->customFields as $customField ) {
                if ( current_user_can( $customField['capability'], $post_id ) ) {
                    if ( isset( $_POST[ $this->prefix . $customField['name'] ] ) && trim( $_POST[ $this->prefix . $customField['name'] ] ) ) {
                        $value = $_POST[ $this->prefix . $customField['name'] ];
                        // 为富文本框的文本自动分段
                        if ( $customField['type'] == "wysiwyg" ) $value = wpautop( $value );
                        update_post_meta( $post_id, $this->prefix . $customField[ 'name' ], $value );
                    } else {
                        delete_post_meta( $post_id, $this->prefix . $customField[ 'name' ] );
                    }
                }
            }
        }
    } // End Class

} // End if class exists statement

// 实例化类
if ( class_exists('myCustomFields') ) {
    $myCustomFields_var = new myCustomFields();
}
{% endcodeblock %}

`wp_verify_nonce`验证了提交的数据是否可信.

## 七、完整的代码

点击[这里](http://fayaa.com/code/view/15185/)查看.
