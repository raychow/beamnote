---
title: 用杂边让 GIF 图像更平滑
tags:
  - 图像
id: 186
categories:
  - 技巧
date: 2010-06-04 22:16:50
---

为了减小图像体积, 更多的时候是为了避免 IE6 不支持 PNG 图像的 Alpha 通道, 保存透明图像时我经常采用 GIF 格式或者只含索引色的 PNG 格式. 虽然网路上有很多使得 IE6 修正对 Alpha 通道支持的方法, 但有些方法只能对 PNG 背景有效, 有些方法要载入 JS, 影响显示速度, 所以我更倾向于直接使用 GIF 图片.

以本站刚刚更换的 Logo 图片为例 (现在我又换回来了) :

[![Logo Snapshot](http://img.beamnote.com/2010/logosnap.jpg)](http://img.beamnote.com/2010/logosnap.jpg)<!-- more -->

白色的 Logo 实际上是一张 GIF 图像. 也许您会疑问, 由于不支持 Alpha 通道, 原本在 Photoshop 中看起来很平滑的图像保存后会出现锯齿, 就像下面这样:

[![Bad Logo Snapshot](http://img.beamnote.com/2010/logobadsnap.jpg)](http://img.beamnote.com/2010/logobadsnap.jpg)

在曲线的地方锯齿就更明显, 可以用这个方法弥补: 在保存 GIF 时, 将杂边选择为与此图片周围背景色相近的颜色. 上例中选择的是绿色.

[![保存索引色](http://img.beamnote.com/2010/saveindex.png)](http://img.beamnote.com/2010/saveindex.png)

此时, 图像边缘会出现刚刚选定的颜色, 就可以与背景融合, 消除锯齿了.
