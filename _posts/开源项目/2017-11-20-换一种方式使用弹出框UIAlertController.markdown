---
layout: post
title:  "换一种方式使用弹出框UIAlertController"
date:   2017-11-20 15:06:31 +0800
categories: 开源项目
---
![](http://yuqiangcoder.com/assets/postImages/ios/201711/8.jpg)

## Github 地址
[UIViewControllerYQAlert](https://github.com/YQqiang/UIViewControllerYQAlert)
## 介绍
在开发过程中, 难免会使用到弹出框; 系统的弹出框感觉不太能满足要求, 第三方的弹出框又不如系统的简洁.
该项目是基于系统的弹出框`UIAlertController`扩展出来可以简单修改样式的`Alert`, 并且以链式调用使用弹出框.

## 功能
- 自定义按钮文字颜色
- 自定义标题文字和描述文字颜色和字体
- 支持在标题顶部增加logo图片
- 支持单个按钮的弹出框
- 优雅的调用方式
- 正则表达式检索关键字, 增加超链接

## 要求
- Xcode 9.0
- Swift 4.0

## 安装
直接把`UIViewControllerYQAlert.swift`文件拖入项目中

## Demo
可直接下载项目运行Demo查看效果

## 使用
> `yq`是对`UIViewController`的扩展, 因此在`ViewController`中显示弹出框时,可直接使用`yq.makeAlert`方式

#### 1. 基本用法

![基本用法.png](http://yuqiangcoder.com/assets/postImages/ios/201711/9.png)

```Swift
yq.makeAlert { (make) in
    make.title = "标题"
    make.desc = "弹出框描述文字弹出框描述文字弹出框描述文字弹出框描述文字弹出框描述文字"
}.show()
```

#### 2. 更改按钮颜色

![更改按钮颜色.png](http://yuqiangcoder.com/assets/postImages/ios/201711/10.png)

```swift
yq.makeAlert { (make) in
    make.title = "提示"
    make.desc = "自定义按钮颜色"
    make.cancelTitleColor = UIColor(colorHex: 0xF47920)
    make.confirmTitleColor = UIColor(colorHex: 0xF47920)
}.show()
```

#### 3. 更改按钮文字

![更改按钮文字.png](http://yuqiangcoder.com/assets/postImages/ios/201711/11.png)

```swift
yq.makeAlert { (make) in
	make.title = "提示"
	make.desc = "自定义按钮文字内容"
	make.cancelTitleColor = UIColor(colorHex: 0xF47920)
	make.confirmTitleColor = UIColor(colorHex: 0xF47920)
	make.cancelTitle = "好吧,暂不更换 😌"
	make.confirmTitle = "我知道了,继续更换 🤣"
}.show()
```

#### 4. 自定义标题和描述文字

![自定义标题和描述文字.png](http://yuqiangcoder.com/assets/postImages/ios/201711/12.png)

```swift
yq.makeAlert { (make) in
    make.title = "警告"
    make.titleFont = UIFont.systemFont(ofSize: 22)
    make.titleColor = UIColor.red
    make.desc = "正在执行非常危险的操作"
    make.descFont = UIFont.systemFont(ofSize: 18)
    make.descColor = UIColor.darkGray
}.show()
```

#### 5. 自定义顶部图片

![自定义顶部图片.png](http://yuqiangcoder.com/assets/postImages/ios/201711/13.png)

```swift
yq.makeAlert { (make) in
    make.desc = "抱歉, 操作失败"
    make.titleImage = UIImage(named: "fail")
    make.cancelTitleColor = UIColor(colorHex: 0xF47920)
    make.confirmTitleColor = UIColor(colorHex: 0xF47920)
}.show()
```

#### 6. 按钮点击事件回调

![按钮点击事件回调.png](http://yuqiangcoder.com/assets/postImages/ios/201711/14.png)

```swift
yq.makeAlert { (make) in
    make.title = "提示"
    make.desc = "耶!!! 操作成功!"
    make.titleImage = UIImage(named: "success")
    make.cancelTitleColor = UIColor(colorHex: 0xF47920)
    make.confirmTitleColor = UIColor(colorHex: 0xF47920)
}.confirmClosure({ (action) in
    print("\(String(describing: action.title))")
}).cancelClosure({ (action) in
    print("\(String(describing: action.title))")
}).show()
```

#### 7. 一个操作按钮的弹出框

![一个操作按钮的弹出框.png](http://yuqiangcoder.com/assets/postImages/ios/201711/15.png)

```swift
yq.makeAlert { (make) in
    make.title = "提示"
    make.desc = "只有一个操作按钮的弹出框"
    make.confirmTitleColor = UIColor(colorHex: 0xF47920)
}.confirmClosure({ (action) in
    print("\(String(describing: action.title))")
}).showSingleConfirm()
```
#### 8. 超链接文本

![超链接文本](http://yuqiangcoder.com/assets/postImages/ios/201711/17.gif)

```swift
yq.makeAlert { (make) in
  make.title = "温馨提示"
  make.desc = "您已连续超过五次输入密码错误, 账号已被锁定, 请点击找回密码进行密码找回."
  make.linkUnderLineStyle = .single
  make.linkColor = UIColor.red
  make.passColor = UIColor.gray
  make.linkRegex = "(?:找回密码)"
  }.linkClickEnd { (label, text) in
      print("-------- \(text)")
      let viewController = UIViewController()
      viewController.title = text
      viewController.view.backgroundColor = UIColor.white
      self.presentedViewController?.dismiss(animated: true, completion: nil)
      self.navigationController?.pushViewController(viewController, animated: true)
}.showSingleRight()
```

#### 9. 确定按钮倒计时结束时才可点击

![超链接文本](http://yuqiangcoder.com/assets/postImages/ios/201711/18.gif)

```swift
yq.makeAlert { (make) in
  make.title = "隐私协议"
  make.desc = "隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议"
  make.rightActionTitle = "我已阅读并同意"
}.showCountdown(5)
```

#### 10. 综合使用

![综合使用.png](http://yuqiangcoder.com/assets/postImages/ios/201711/16.png)

```swift
yq.makeAlert { (make) in
    make.title = "提示"
    make.desc = "自定义按钮文字内容"
    make.titleImage = UIImage(named: "fail")
    make.cancelTitleColor = UIColor(colorHex: 0xF47920)
    make.confirmTitleColor = UIColor.gray
    make.cancelTitle = "好吧,暂不更换 😌"
    make.confirmTitle = "我知道了,继续更换 🤣"
}.confirmClosure({ (action) in
    print("\(String(describing: action.title))")
}).cancelClosure({ (action) in
    print("\(String(describing: action.title))")
}).show()
```

## Github 地址
[UIViewControllerYQAlert](https://github.com/YQqiang/UIViewControllerYQAlert)

## 联系我：
- 博客: http://yuqiangcoder.com/
- 邮箱: yuqiang.coder@gmail.com

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


