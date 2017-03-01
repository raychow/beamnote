---
title: shared_ptr 是否线程安全?
tags:
  - C++
  - 线程安全
id: 483
categories:
  - 唠叨
date: 2014-09-25 16:58:28
---

在上周参加的一场面试中, 面试官和我聊到了智能指针的问题, 问我常用哪些智能指针, 我说出了 `shared_ptr`、`unique_ptr`, 脑子一抽风把 `scoped_ptr` 也说出来了, 但是把 `weak_ptr` 和 `intrusive_ptr` 忘在了脑后…

[![](//img.beamnote.com/2014/thread.jpg)](//img.beamnote.com/2014/thread.jpg)<!-- more -->

他问我了一个问题, 对于 `shared_ptr` 的操作是否为线程安全的, 当时庆幸这个问题之前有做过了解, 就按照某个博客的说法来回答: 尽管使用计数提供了原子性修改操作, 但 `shared_ptr` 的赋值操作由复制对象指针和修改使用计数两个操作复合而成, 因此仍不是线程安全的, 具体理由如下.

设有两个线程与三个智能指针:

{% codeblock lang:cpp %}
thread thread1;
thread thread2;
shared_ptr<Object> spObject1;
shared_ptr<Object> spObject2;
shared_ptr<Object> spObject3;
{% endcodeblock %}

在某时刻, spObject1 与 spObject2 的状态是这样的:

[![](//img.beamnote.com/2014/shared_ptr_1.png)](//img.beamnote.com/2014/shared_ptr_1.png)

两个线程同时进行了 `shared_ptr` 的赋值操作:

{% codeblock lang:cpp %}
// thread1
spObject1 = spObject2;

// thread2
spObject2 = spObject3;
{% endcodeblock %}

观察 `spObject1` 与 `spObject2` 的操作, 首先 `thread1` 执行, `spObject1` 的对象指针指向了 `spObject2` 指向的位置:

[![](//img.beamnote.com/2014/shared_ptr_2.png)](//img.beamnote.com/2014/shared_ptr_2.png)

接着 `thread2` 执行, `spObject2` 的指针指向了 `spObject3` 指向的的位置, 并且修改使用计数, 导致 `object2` 与相应的使用计数被销毁, `spObject1` 指向了一个被销毁的对象:

[![](//img.beamnote.com/2014/shared_ptr_3.png)](//img.beamnote.com/2014/shared_ptr_3.png)

`thread1` 执行时, `spObject1` 修改使用计数, 错误的指向了原本 `spObject3` 的使用计数.

[![](//img.beamnote.com/2014/shared_ptr_4.png)](//img.beamnote.com/2014/shared_ptr_4.png)

据此原博得出结论, 多线程无保护读写 `shared_ptr` 是不安全的, 必须加锁. 当我把这个过程呈现给面试官的时候, 他表示了疑惑: 既然 `shared_ptr` 的读写不是线程安全的, 那么使用计数操作的原子性意义何在? 我当时就囧了, 但仍然坚持原来的主张, 于是面试官便让我回去查查.

首先我阅读了 VC 2013 的 STL 库, 对于赋值部分看起来是线程安全的, 这里是赋值的源码:

{% codeblock lang:cpp %}
  _Myt&amp; operator=(const _Myt&amp; _Right) _NOEXCEPT
    {  // assign shared ownership of resource owned by _Right
    shared_ptr(_Right).swap(*this);
    return (*this);
    }

  void swap(_Myt&amp; _Other) _NOEXCEPT
    {  // swap pointers
    this->_Swap(_Other);
    }
{% endcodeblock %}

`swap` 交换是实现异常安全的惯用手法, 不难理解. `_Swap` 声明在基类 `_Ptr_base<_Ty>` 中, 具体如下:

{% codeblock lang:cpp %}
  void _Swap(_Ptr_base&amp; _Right)
    {  // swap pointers
    _STD swap(_Rep, _Right._Rep);
    _STD swap(_Ptr, _Right._Ptr);
    }
{% endcodeblock %}

此处注意首先交换了使用计数, 然后是对象指针. 我们再看 `shared_ptr` 的复制构造函数, 因为赋值语句的第一行用到了它:

{% codeblock lang:cpp %}
  shared_ptr(const _Myt&amp; _Other) _NOEXCEPT
    {  // construct shared_ptr object that owns same resource as _Other
    this->_Reset(_Other);
    }

  template<class _Ty2>
    void _Reset(const _Ptr_base<_Ty2>&amp; _Other)
    {  // release resource and take ownership of _Other._Ptr
    _Reset(_Other._Ptr, _Other._Rep);
    }

  void _Reset(_Ty *_Other_ptr, _Ref_count_base *_Other_rep)
    {  // release resource and take _Other_ptr through _Other_rep
    if (_Other_rep)
      _Other_rep->_Incref();
    _Reset0(_Other_ptr, _Other_rep);
    }
{% endcodeblock %}

可以注意到, 在泛型函数的 `_Reset` 中, 已经取得了一份 `_Other` 的拷贝, 也就是说争用情况不会发生在此之后, 只会在泛型版本 `_Reset` 调用 非泛型版本 `_Reset` 时发生.

下面做一个简单的实验证明上面的结论:

{% codeblock lang:cpp %}
        shared_ptr<int> sp1(new int);
        shared_ptr<int> sp2(new int);
        shared_ptr<int> sp3(new int);
        thread t1([&amp;](){
            while (true)
            {
                sp1 = sp2;
                sp1.reset(new int);
            }
        });
        thread t2([&amp;](){
            while (true)
            {
                sp2 = sp3;
                sp3.reset(new int);
            }
        });
        t1.join();
        t2.join();
{% endcodeblock %}

此程序在 `Debug` 模式下运行不久就会出现错误, 因为尝试越界访问 (可能是写入一个已经销毁的堆上对象).

[![](//img.beamnote.com/2014/heap_corruption.png)](//img.beamnote.com/2014/heap_corruption.png)

令人疑惑的是, [cppreference.com](http://en.cppreference.com/w/cpp/memory/shared_ptr) 指出, 对不同的 `shared_ptr` 实例操作时是不需要额外同步的, 即使那些实例共享了同一个对象. 在上面的实验中, 两个线程只进行以下操作:

* 对不同的实例调用成员方法 `operator=`;
* 对不同的实例调用成员方法 `reset`.

对于 VC 2013 的 STL 库, 以下的执行顺序是可能的:

1. 线程 `t1` 中, 泛型 `_Reset` 取得了 <sp2._Rep;
2. 线程 `t2` 中, 完成了 sp2 = sp3, sp2._Rep 与 sp2._Ptr 被销毁;
3. 线程 `t1` 中, 泛型 `_Reset` 取得了 sp2._Ptr (实际上已经是 sp3._Ptr), 调用非泛型 `_Reset`, 对已销毁的 sp2._Rep 进行操作, 发生错误.

在 gcc 中测试, 也会发生类似的问题.

至此, 我仍然坚持 `shared_ptr` 并非线程安全这一观点, 起码对于现在的库来说是不安全的.
