---
title: "Haskell 学习札记 (一) : Church Numerals"
tags:
  - Haskell
  - 编程
id: 520
categories:
  - 分享
date: 2015-03-13 05:50:07
---

在跟着 [FP101x](https://courses.edx.org/courses/DelftX/FP101x/3T2014/info) 学习的时候, 一直感觉良好, 老师虽然有点背书的样子但还是学到了新东西. 直到高阶函数那一章, 某个上课会紧张的呆萌助教小哥出现, 开口就蹦出了 Church Numerals, 接着「卧槽什么鬼」的东西出现了, 十来分钟的课程听的云里雾里最后也没弄明白他在说啥. 课后我去搜维基, 维基解释 Church 数是数据和运算符嵌入到 lambda 演算内的一种方式, 我就更糊涂了. 幸运的是, 我找到了 [Church numerals: a tutorial](https://karczmarczuk.users.greyc.fr/Essays/church.html) 这篇文章, 下面就结合我的理解来谈谈 Church Numerals.

[![Haskell](//img.beamnote.com/2015/haskell.png)](//img.beamnote.com/2015/haskell.png)<!-- more -->

## 一、Church Numerals

最大的问题是, Church Numerals 究竟是什么? 其实就像助教小哥举得那个例子, 我们有很多种方式可以计数, 例如使用阿拉伯数字, 例如 `4`, 也可以用二进制, 例如 `0b100`, 甚至是用星号表示, 例如 `****`. 既然有这么多种方式表示数字, 那么在算数系统中有没有一种中性的、尽可能抽象的方法来表示数字呢? Church Numerals 就是一种在函数式框架下实现的表示数字的模型.

Church Numbers 基于两个参数: `zero`, 就表示 0, 我理解成数字的基准, 以及 `successor`, 用于求数字后继的函数, 例如 `successor 4 == 5`, `successor "****" == "*****"`, 等等. 所以, 数字 _n_ 可以表示为

`n s z`

其中, s 与 z 分别是 `successor` 与 `zero`. 举个实际的例子, 数字 1 就可以表示为 `(+1) 0`.

那么, 用 Church Numerals 该怎样表示数字呢? 我们有

{% codeblock %}
zero s z = z
one s z = s z
two s z = (s . s) z
three s z = (s . s . s) z
{% endcodeblock %}

其实也不难理解对吧, 零就直接返回表示零的东西好了, 一的话取得零的后继就是了, 二的话得取零的后继两次, 以此类推. 当然在 Haskell 里面会更喜欢这样写:

{% codeblock %}
zero = \f -> id -- zero s = id
one = \f -> f -- one s = s
two = \f -> f . f -- two s = s . s
three = \f -> f . f . f -- three s = s . s . s
{% endcodeblock %}

其中 `zero` 中的 `id` 是 `Prelude` 里的函数, 它的作用是接收啥就返回啥, 主要作用是为了让代码更简洁 (取代 `\x -> x`, 所以 `one` 定义成 `one = id` 也是没有问题的), `zero = \f -> id` 扔掉了第一个参数, 直接返回后面的东西, 也就是 `z`.

## 二、基本运算操作

有了数字, 我们就能在上面进行一些基本运算.

### 1\. 自增

最最基本的, 是自增操作, 在 Haskell 里可以定义的五花八门:

{% codeblock %}
incr n s z = n s (s z)
           = s (n s z)
{% endcodeblock %}

各种定义有各种解释, 第一种是说把基准值往上自增了, 然后再取后继, 第二种是说直接在数字上取后继, 但 _n_ (_s_ . _s_) _z_ 就不对, 因为这相当于把取后继的速度加快了一倍, 变成了取两倍的函数.

我们可以把它变得更符合 Haskell 精神:

{% codeblock %}
incr n s = n s . s
         = s . n s
{% endcodeblock %}

证明 `n s . s = s . n s`, 感觉是句废话, 但又不好证, 乱写一通:

{% codeblock %}
n s . s = (s . s . ... . s) [n 个 s] . s
        = s . s . ... . s [n+1 个 s]
        = s . (s . s . ... . s) [n 个 s]
        = s . n s
{% endcodeblock %}

### 2\. 加法

加法操作的第一个定义很符合直觉:

{% codeblock %}
add n m = n incr m
{% endcodeblock %}

看起来就是 _n_ 加上 _m_, 到底是怎么回事呢? 实际上 `incr` 成了后继函数, 而 _m_ 成了零, `n incr m` 就是对着 _m_ 调用了 _n_ 次 `incr`. 按着这个理解, 就有了第二种形式:

{% codeblock %}
(add n m s) z = n s $ m s z
{% endcodeblock %}

也即

{% codeblock %}
add n m s = n s . m s
{% endcodeblock %}

证明 `add (incr n) m = add n (incr m)`:

{% codeblock %}
add (incr n) m s = (incr n) s . m s
               = (s . n s). m s
               = (n s . s). m s
               = n s . (s . m s)
               = n s . (incr m) s
               = add n (incr m) s
{% endcodeblock %}

### 3\. 乘法

还记得刚刚说 `n (s . s) z` 不是加法而是乘法了吗, 因为我们把后继函数加快了一倍, 如果我们放 _m_ 个 _s_ 进去, 变成 `n (m s) z`, 实际上就是 _n_ 与 _m_ 相乘了, 是不是很神奇?

{% codeblock %}
mul n m s z = n (m s) z
{% endcodeblock %}

或者是:

{% codeblock %}
mul n m = n . m
{% endcodeblock %}

上面这行实际上是先把 `s z` 给 _m_ 处理, 得到 `(m s) z`, 再丢给 _n_ 处理.

证明 `a*(b+c)=a*b+a*c`:

{% codeblock %}
mul a (add b c) s = a (add b c s)
                  = a (b s . c s)
                * = a (b s). a (c s)
                  = (mul a b) s . (mul a c) s
                  = add (mul a ) (mul a c) s
{% endcodeblock %}

注<sup>*</sup>: 不清楚等号是否在所有情况均成立, 但在 Church Numerals 的前提下是成立的.

### 4\. 乘方

乘方的形式与乘法非常接近, 就是:

{% codeblock %}
pow n m = m n
{% endcodeblock %}

如何去理解? 先把乘方补完:

{% codeblock %}
pow n m s z = m n s z
       = (m n s) z
            = (n . n . ... . n) s z
        = (mul n (mul n ( .. (mul n n)))) s z
{% endcodeblock %}

可以看出, 是把乘以 _n_ 的操作调用了 _m_ 次, 也即乘方.
甚至可以用它推出一些定理:

{% codeblock %}
pow n (incr m) = incr m n
               = m n . n
               = mul (pow n m) n
{% endcodeblock %}

因此得到了 _n<sup>m</sup>_+1 = _n<sup>m</sup>_*_n_.
