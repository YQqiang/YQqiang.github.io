---
layout: post
title:  "BgImgTextView"
date:   2017-03-20 10:17:31 +0800
categories: 开发笔记
---
![](http://upload-images.jianshu.io/upload_images/3538284-86506572fc2c26e6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 半个月没有更新博客了, 俗话说 ***三天不学习赶不上刘少奇*** ; 这半个月又不知被别人甩出去多远😂😂😂

说说正题吧~~~~

在改项目中的bug时, 发现`TextView`的内容不能设置滑动的内边距, 以致于给textView添加背景图片后, 内容滑动会超出背景图片; 项目中的`textView` 的封装是继承自`UITextView` , 改起来比较棘手~~(是我不知道如何去改, 嘘.......)~~, 所以决定手撸一个.

> ps: 有大神知道如何设置`textView`滑动的内边距, 还望指教, 非常感谢.
本人菜鸟一枚, 才疏学浅, 望大神们多指教.

## bug演示
![bug](http://upload-images.jianshu.io/upload_images/3538284-c9f9191fc4441220.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## BgImgTextView
1. 带有占位文字(`placeholder`)
2. 可以添加背景图片(`bgImgName` / `bgImg`)
3. 可以给内容设置内边距(`edgeInsets`), 使其内容滑动时不超过背景图片

## 效果展示

![BgImgTextView.gif](http://upload-images.jianshu.io/upload_images/3538284-3bed97a0cfc42175.gif?imageMogr2/auto-orient/strip)

## github地址
[BgImgTextView](https://github.com/YQqiang/BgImgTextView)

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


