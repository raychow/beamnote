---
title: Haskell 学习札记 (序)
tags:
  - Haskell
id: 516
categories:
  - 分享
date: 2015-03-05 03:37:05
---

初学 Haskell 不久, 惊叹函数式编程的强大、优雅之余, 更多是深深地受虐于 Haskell 的大坑小坑中. 本是秉着「程序员应该要学习至少一门面向对象语言、一门函数式语言, 以及一门动态语言」的宗旨, 想对 Haskell 做些简单的了解, 谁曾想随着 [Learn you a Haskell](http://learnyouahaskell.com/) 的深入阅读, 我愈发喜爱这门语言. 于是, 正要八经的学下去吧. 为明此志, 决心开始撰写学习札记, 也算为博客注入活力了.

[![Haskell](//img.beamnote.com/2015/haskell.png)](//img.beamnote.com/2015/haskell.png)<!-- more -->

## 一、初学者

作为一名初学者, 利用以下资源学习会比较快上手.

1. 首先自然是刚刚才提到的 [Learn you a Haskell](http://learnyouahaskell.com/), 用比较有趣的方式启蒙了很多 Haskell 使用者;
如果是英文苦手, 这本汉化的 [Haskell 趣學指南](http://learnyouahaskell-zh-tw.csie.org/)会让你轻松很多.
2. 在学习过程有函数不明白的, [Hoogle](https://www.haskell.org/hoogle/) 可以帮到你, 这是 Haskell 的 API 搜索引擎. 感觉这个名字真是风趣幽默 :-).
3. [H-99: Ninety-Nine Haskell Problems](https://wiki.haskell.org/99_questions) 里面有八十八道函数式编程练习题, 因为是从 Prolog 弄过来的, 一些题目其实在 Haskell 里面已经天然的解决了. 我在读完了 Learn you a Haskell 以后做这个练习, 大部分题目还是很轻松的, 但一些涉及到 Monad 的题目真的很难理解, 需要更加精进才行.
4. 接着可以阅读 [Real World Haskell](http://book.realworldhaskell.org/) 了, 这本书的确非常的实在, 每一个点都会配一个例子, 从命令式编程转过来的新手们只有多多练习才能适应函数式编程思维. 我现在还没有看.
5. 之所以还没有看上面这本书, 是因为被 `Monad` 弄得头大, 正在研读教程 [All About Monads](https://wiki.haskell.org/All_About_Monads).
6. 也可以跟着时下流行的 MOOC 课堂进行学习, 例如 [FP101x](https://courses.edx.org/courses/DelftX/FP101x/3T2014/info). 我是第一次上 MOOC 的课程, 才刚刚看完几节课而已, 好坏以后再评.
7. 再有以后, 跟着 Stack Overflow 上的这则[回答](http://stackoverflow.com/a/1016986/1453917)继续学习吧.

## 二、IDE

感觉 Haskell 没有什么特别好的 IDE 可用, 试过一些, 列举如下.

1. 文字编辑器 + GHCi
试了很多, 只有这个最顺手, 毕竟学习阶段写的代码都是小段小段的. 文字编辑器中 Sublime 最为推荐, 原生提供了对 Haskell 的支持, 不仅有代码高亮, 保存的时候也会顺手帮你编译一遍, 常常提示你一堆问题, 再配合 [SublimeHaskell](https://github.com/SublimeHaskell/SublimeHaskell) 插件就更好用了.
2. FPComplete
线上的 Haskell IDE, 赏心悦目的, 很不错. 按 `Ctrl + 空格` 进行补全, 当然在中文输入法下没用.
3. Leksah
一个跨平台的 Haskell IDE, 平时用 Visual Studio 多了, 觉得这个不够好用.
4. EclipseFP plugin for Eclipse IDE
说实话吧, 网络问题, 根本没装起来.

## 三、学习感想

学 Haskell, 功利心不可以太重, 当然还是可以去官方看看有哪些公司在用 Haskell: [Haskell in industry](https://wiki.haskell.org/Haskell_in_industry).

私以为, 学习 Haskell 就是为了开拓视野, 从不一样的角度去看问题. 虽然说语言被划分了命令式与函数式的界限, 但函数式编程已经渗透到很多命令式编程语言当中, 只要有 Lambda 演算的地方, 函数式编程的知识就可以用到. 特别是函数式编程的一些特性, 例如 Immutable Data、First Class Function、Lazy Evaluation, 都极大的改变了传统的编程观: 不能改变数据的值, 该如何表示一个不断演进的计算? 一批数据需要进行处理, 怎样做才简洁高效? 明明看起来是无穷递归的嵌套引用, 为什么计算在运行时可以终止?

现在我又想到那个困扰我的 `Monad` 了, `Monad` 是 Haskell 中以某种方式合并计算结果以进行更加复杂运算的一种策略, 使用神奇的 `Monad` 配合 `do` 语法糖, 就能让 Haskell 看起来像命令式语言那样, 对「变量」赋值并运算, 就像下面这样:

{% codeblock %}
do
    x <- [1, 2]
    y <- [3, 4]
    return (x, y)
{% endcodeblock %}

看完 Learn you a Haskell 里面的 `Monad` 时, 我处于非常迷糊的状态, 只能非常机械的理解: 喔, `x` 与 `y` 表示了两个不确定的值, 在 `return (x, y)` 的时候 Haskell 也不知道你究竟要什么, 只好把所有的组合都给你列出来啦, 也就是 `[(1,3),(1,4),(2,3),(2,4)]`. 那么问题来了, 究竟是 Haskell 的什么机理让它这样执行? 通过这几天持续的学习, 渐渐对上面的几行代码有了新的理解. 我们知道, `do` 表示法与以下式子等价:

{% codeblock %}
[1,2] >>= \x -> [3,4] >>= \y -> return (x, y)
{% endcodeblock %}

下面的处理就要归功于 `bind` 这种神奇的函数了, `List Monad` 的定义如下:

{% codeblock %}
instance Monad [] where
    return x = [x]
    xs >>= f = concat (map f xs)
    fail _ = []
{% endcodeblock %}

原来在 [1, 2] >>= f 的时候, 会把 [1, 2] 里的每个值都拿出来调用 f, 而 [3, 4] >>= f' 的时候也会把 [3, 4] 里的每个值都拿出来调用 f', 所以才会看到所有的值两两组合. 同时, 最后的 `return` 还进行了类型推导, 不由得让我惊叹 Haskell 类型系统的精妙.

其实 Learn you a Haskell 里面也有提到这些, 但在当时阅读这些文字是绝对无法体会到为什么能这样做、为什么要这样做的, 唯有经过自己不断地练习, 去反复理解每一句话的执行机理, 才能慢慢领会各种奥秘.

我想到了某个 Haskell 讨论帖中, 一位 Haskell 初心者的回帖:
> 你这个代码写的不错, 我研究了一个月, 终于看明白了. 谢谢\! -- 此处省略两个感叹号.
是怎么回事呢? 楼主贴了一段用 Haskell 求数列组合的代码, 当时把这段寥寥数字的代码看懂了, 暗自认为这个人花一个月看懂也未免太过夸张, 觉得很好笑. 直到做 H-99 碰到一些用 Haskell 优雅解出的题目, 自己却很难看明白, 便明白 Haskell 当真是字字珠玑, 要想吃透 (甚至仅仅上手) 实属不易.

所以说, Haskell 的学习曲线是比较陡峭的, 尤其对于之前没有接触过函数式编程的人. 套用 [Erik Meijer](https://twitter.com/headinthebox) 老师的梗, 得要 on and on: wax on wax off, wax on, wax off…直到形成肌肉记忆, 便能手到擒来.
