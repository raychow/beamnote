---
title: 没有 JavaScript, 你的网页是啥样?
tags:
  - JavaScript
  - 博客
  - 网络
id: 266
categories:
  - 技巧
date: 2010-11-11 21:14:52
---

现在很多网站使用了 JavaScript, 它让交互响应更加灵敏, 也增强了所谓的「用户体验」. 尽管它是一门很简单的程序设计语言, 但由于现在 Web 应用程序越来越多, JavaScript 的地位也变得越来越重要. 不过, 在享受 JavaScript 和衍生的库的同时, 你想过如果没有 JavaScript, 你现在的网页看起来是什么样子?

[![JavaScript](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/without-javascript.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/without-javascript.png)<!-- more -->

我的博客使用了 jQuery 的一些功能, 显然它是基于 JavaScript 的. 我关掉了浏览器的 JavaScript 引擎, 打开博客, 幸好没有变形——广告无法显示, 这不是我能控制的.

[![没有 JavaScript 的光线誌](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/beamnote-without-js.png)](//beamnote-img.oss-cn-shanghai.aliyuncs.com/2010/beamnote-without-js.png)

「文章」菜单不能显示, 因为我是通过 jQuery 注册 hover 事件实现弹出的; 「回顶部」的按钮也无效了, 当然这影响不大.

在完成应用 JavaScript 之前 (或者之后? ), 我们应该思考一下如何聪明地使用 JavaScript , 从而确保不会因为它的缘故使任何人无法访问你的网站. 这就是非干扰性的 JavaScript 背后的核心理念.

为了更好地理解什么是非干扰性 JavaScript 以及我们为什么需要它, 我们先来看看在 JavaScript 程序设计中会碰到哪些不确定性因素:

* 因为某些浏览器不支持 JavaScript 或它们的支持的 JavaScript 版本太老了, 这些浏览器可能会完全忽略你的脚本;
* 即使一个浏览器可以支持 JavaScript , 用户们也可能会出于安全性的考虑而禁用 JavaScript , 用户所在公司的防火墙也可能会通过移除所有的 `<script>` 标签来阻止 JavaScript 的运行;
* 即便一个浏览器支持 JavaScript , 它也可能不理解 DOM 规范中的某些浏览器专有特性 ( IE 经常被认为是这方面的祸首), 这会导致该浏览器无法理解你的部分脚本;
* 即便是脚本能得到正确地解释, 它也可能要依赖于非常复杂的 HTML , 并且 / 或者依赖于可能以无法预测的方式被改变的 HTML ;
* 就算 JavaScript 脚本运行在完美无误的 HTML 中, 你也不能确定你的用户将会用什么类型的输入设备. 许多脚本只有在用户使用鼠标时才会生效, 而对那些使用键盘的人群则没有反应 (许多残障用户无法使用鼠标, 而且有的人偏偏喜欢用键盘);
* 即使你的脚本回避了上述的所有风险而且运行得很好, 其他的程序员们也有可能读不懂它.

上面一大段总结的很好, 因为它是抄袭的 :-| , 它完善的表达了我想叙述的原因. 如果网页中的关键内容是由 JavaScript 控制的, 一部分用户将不能正常的使用你的网站, 或者丢失重要讯息.

如果某些内容由 JavaScript 控制, 在 JavaScript 无法正常工作的时候, 我的方法是通过 CSS 设定好样式, 让他看起来是正常的.

这里有一个 jQuery 滑动导航条的例子 ([Demo1](/demo/nojs/jquery-list.html), [Demo2](/demo/nojs/jquery-list-fix.html)), Demo1 乍看之下是正常的, 但如果关闭了 JavaScript, 所有的项目会挤到一团, 因为所有的列表项都是绝对定位的, 需要由 JavaScript 运行时动态确定高度; Demo2 中只做出了简单的修正, 使得它在 JavaScript 运行前保持相对定位, 就能使它在没有 JavaScript 的状况下表现正常.

这个例子尽管简单, 但有时候我们就是为了方便 (也是假定了 JavaScript 永远工作) 让 JavaScript 参与重要元素的排版, 这是要尽量避免的.
