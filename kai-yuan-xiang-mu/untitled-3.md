# 换一种方式使用弹出框UIAlertController \| 大道至简 悟在天成

![](http://yuqiangcoder.com/assets/postImages/ios/201711/8.jpg)

## Github 地址 <a id="github-&#x5730;&#x5740;"></a>

[UIViewControllerYQAlert](https://github.com/YQqiang/UIViewControllerYQAlert)

## 介绍 <a id="&#x4ECB;&#x7ECD;"></a>

在开发过程中, 难免会使用到弹出框; 系统的弹出框感觉不太能满足要求, 第三方的弹出框又不如系统的简洁. 该项目是基于系统的弹出框`UIAlertController`扩展出来可以简单修改样式的`Alert`, 并且以链式调用使用弹出框.

## 功能 <a id="&#x529F;&#x80FD;"></a>

* 自定义按钮文字颜色
* 自定义标题文字和描述文字颜色和字体
* 支持在标题顶部增加logo图片
* 支持单个按钮的弹出框
* 优雅的调用方式
* 正则表达式检索关键字, 增加超链接
* 确定按钮倒计时结束后才可点击

## 要求 <a id="&#x8981;&#x6C42;"></a>

* Xcode 9.0
* Swift 4.0

## 安装 <a id="&#x5B89;&#x88C5;"></a>

直接把`UIViewControllerYQAlert.swift`文件拖入项目中

## Demo <a id="demo"></a>

可直接下载项目运行Demo查看效果

## 使用 <a id="&#x4F7F;&#x7528;"></a>

> `yq`是对`UIViewController`的扩展, 因此在`ViewController`中显示弹出框时,可直接使用`yq.makeAlert`方式

### 1. 基本用法 <a id="1-&#x57FA;&#x672C;&#x7528;&#x6CD5;"></a>

![&#x57FA;&#x672C;&#x7528;&#x6CD5;.png](http://yuqiangcoder.com/assets/postImages/ios/201711/9.png)

```text
yq.makeAlert { (make) in
    make.title = "标题"
    make.desc = "弹出框描述文字弹出框描述文字弹出框描述文字弹出框描述文字弹出框描述文字"
}.show()
```

### 2. 更改按钮颜色 <a id="2-&#x66F4;&#x6539;&#x6309;&#x94AE;&#x989C;&#x8272;"></a>

![&#x66F4;&#x6539;&#x6309;&#x94AE;&#x989C;&#x8272;.png](http://yuqiangcoder.com/assets/postImages/ios/201711/10.png)

```text
yq.makeAlert { (make) in
    make.title = "提示"
    make.desc = "自定义按钮颜色"
    make.cancelTitleColor = UIColor(colorHex: 0xF47920)
    make.confirmTitleColor = UIColor(colorHex: 0xF47920)
}.show()
```

### 3. 更改按钮文字 <a id="3-&#x66F4;&#x6539;&#x6309;&#x94AE;&#x6587;&#x5B57;"></a>

![&#x66F4;&#x6539;&#x6309;&#x94AE;&#x6587;&#x5B57;.png](http://yuqiangcoder.com/assets/postImages/ios/201711/11.png)

```text
yq.makeAlert { (make) in
	make.title = "提示"
	make.desc = "自定义按钮文字内容"
	make.cancelTitleColor = UIColor(colorHex: 0xF47920)
	make.confirmTitleColor = UIColor(colorHex: 0xF47920)
	make.cancelTitle = "好吧,暂不更换 😌"
	make.confirmTitle = "我知道了,继续更换 🤣"
}.show()
```

### 4. 自定义标题和描述文字 <a id="4-&#x81EA;&#x5B9A;&#x4E49;&#x6807;&#x9898;&#x548C;&#x63CF;&#x8FF0;&#x6587;&#x5B57;"></a>

![&#x81EA;&#x5B9A;&#x4E49;&#x6807;&#x9898;&#x548C;&#x63CF;&#x8FF0;&#x6587;&#x5B57;.png](http://yuqiangcoder.com/assets/postImages/ios/201711/12.png)

```text
yq.makeAlert { (make) in
    make.title = "警告"
    make.titleFont = UIFont.systemFont(ofSize: 22)
    make.titleColor = UIColor.red
    make.desc = "正在执行非常危险的操作"
    make.descFont = UIFont.systemFont(ofSize: 18)
    make.descColor = UIColor.darkGray
}.show()
```

### 5. 自定义顶部图片 <a id="5-&#x81EA;&#x5B9A;&#x4E49;&#x9876;&#x90E8;&#x56FE;&#x7247;"></a>

![&#x81EA;&#x5B9A;&#x4E49;&#x9876;&#x90E8;&#x56FE;&#x7247;.png](http://yuqiangcoder.com/assets/postImages/ios/201711/13.png)

```text
yq.makeAlert { (make) in
    make.desc = "抱歉, 操作失败"
    make.titleImage = UIImage(named: "fail")
    make.cancelTitleColor = UIColor(colorHex: 0xF47920)
    make.confirmTitleColor = UIColor(colorHex: 0xF47920)
}.show()
```

### 6. 按钮点击事件回调 <a id="6-&#x6309;&#x94AE;&#x70B9;&#x51FB;&#x4E8B;&#x4EF6;&#x56DE;&#x8C03;"></a>

![&#x6309;&#x94AE;&#x70B9;&#x51FB;&#x4E8B;&#x4EF6;&#x56DE;&#x8C03;.png](http://yuqiangcoder.com/assets/postImages/ios/201711/14.png)

```text
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

### 7. 一个操作按钮的弹出框 <a id="7-&#x4E00;&#x4E2A;&#x64CD;&#x4F5C;&#x6309;&#x94AE;&#x7684;&#x5F39;&#x51FA;&#x6846;"></a>

![&#x4E00;&#x4E2A;&#x64CD;&#x4F5C;&#x6309;&#x94AE;&#x7684;&#x5F39;&#x51FA;&#x6846;.png](http://yuqiangcoder.com/assets/postImages/ios/201711/15.png)

```text
yq.makeAlert { (make) in
    make.title = "提示"
    make.desc = "只有一个操作按钮的弹出框"
    make.confirmTitleColor = UIColor(colorHex: 0xF47920)
}.confirmClosure({ (action) in
    print("\(String(describing: action.title))")
}).showSingleConfirm()
```

### 8. 超链接文本 <a id="8-&#x8D85;&#x94FE;&#x63A5;&#x6587;&#x672C;"></a>

![&#x8D85;&#x94FE;&#x63A5;&#x6587;&#x672C;](http://yuqiangcoder.com/assets/postImages/ios/201711/17.gif)

```text
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

### 9. 确定按钮倒计时结束时才可点击 <a id="9-&#x786E;&#x5B9A;&#x6309;&#x94AE;&#x5012;&#x8BA1;&#x65F6;&#x7ED3;&#x675F;&#x65F6;&#x624D;&#x53EF;&#x70B9;&#x51FB;"></a>

![&#x8D85;&#x94FE;&#x63A5;&#x6587;&#x672C;](http://yuqiangcoder.com/assets/postImages/ios/201711/18.gif)

```text
yq.makeAlert { (make) in
  make.title = "隐私协议"
  make.desc = "隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议隐私协议"
  make.rightActionTitle = "我已阅读并同意"
}.showCountdown(5)
```

### 10. 综合使用 <a id="10-&#x7EFC;&#x5408;&#x4F7F;&#x7528;"></a>

![&#x7EFC;&#x5408;&#x4F7F;&#x7528;.png](http://yuqiangcoder.com/assets/postImages/ios/201711/16.png)

```text
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

## Github 地址 <a id="github-&#x5730;&#x5740;-1"></a>

[UIViewControllerYQAlert](https://github.com/YQqiang/UIViewControllerYQAlert)

## 联系我： <a id="&#x8054;&#x7CFB;&#x6211;"></a>

* 博客: http://yuqiangcoder.com/
* 邮箱: yuqiang.coder@gmail.com

