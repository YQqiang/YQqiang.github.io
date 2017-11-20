---
layout: post
title:  "Swift 设置UIButton选中状态(UIControlState.selected)且高亮(UIControlState.highlighted)时的图片"
date:   2017-11-17 09:11:31 +0800
categories: 开发笔记
---
![](http://upload-images.jianshu.io/upload_images/3538284-17446c8e61636f90.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 需求
设计师给了四类图片, 普通状态显示加号(+)图片, 按下时显示加号高亮图片, 选中时显示减号(-)图片, 选中按下时显示减号高亮图片.
* 普通状态
* 高亮状态
* 选中状态
* 选中-高亮状态

![normal.png](http://upload-images.jianshu.io/upload_images/3538284-66b0456478b7fc2e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![highlight.png](http://upload-images.jianshu.io/upload_images/3538284-aac75239aa34254c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![select_normal.png](http://upload-images.jianshu.io/upload_images/3538284-536aff019278753e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![select_highlight.png](http://upload-images.jianshu.io/upload_images/3538284-2c2d25a0095be22f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 使用`UIButton`实现上述效果
代码大致如下:
```
button.setImage(UIImage(named: "normal"), for: .normal) 
button.setImage(UIImage(named: "selected_normal"), for: .selected)
button.setImage(UIImage(named: "highlighted"), for: .highlighted)
button .addTarget(self, action: #selector(buttonAction(sender:)), for: .touchUpInside)

@objc fileprivate func buttonAction(sender: UIButton) {
    sender.isSelected = !sender.isSelected
}
```

### 问题
> 加号状态按下时, 可正常显示加号高亮图片; 当减号状态按下时显示系统灰色的高亮加号图片

![2017-11-17 09_00_33.gif](http://upload-images.jianshu.io/upload_images/3538284-46715c3f1bef90b4.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/620)

### 解决
> 设置选中且高亮状态时的图片

```
button.setImage(UIImage(named: "selected_highlighted"), for: UIControlState.selected.union(.highlighted))
```
效果如下:
![2017-11-17 09_07_56.gif](http://upload-images.jianshu.io/upload_images/3538284-4ade2b28f7e336eb.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/620)

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

