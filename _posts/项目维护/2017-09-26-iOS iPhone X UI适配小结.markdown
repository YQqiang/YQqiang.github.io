---
layout: post
title:  "iPhone X UI适配小结"
date:   2017-09-26 20:01:31 +0800
categories: 项目维护
---
![](http://yuqiangcoder.com/assets/postImages/ios/201709/2.png)
### 2017-10-07 更新
[苹果官方中文版iPhone X 适配指南](https://developer.apple.com/cn/ios/update-apps-for-iphone-x/)

---

### 启动图片 launchImage
1. 若使用`LaunchScreen.storyboard`, 则自动会拉伸至全屏幕
![使用LaunchScreen.storyboard](http://yuqiangcoder.com/assets/postImages/ios/201709/3.png)

2. 若使用`Launch Images source`
![使用Launch Images source](http://yuqiangcoder.com/assets/postImages/ios/201709/4.png)
需要一张`1125px × 2436px`分辨率的资源文件

![需要的资源文件](http://yuqiangcoder.com/assets/postImages/ios/201709/5.png)

![资源文件尺寸](http://yuqiangcoder.com/assets/postImages/ios/201709/6.png)

### TabBar 高度
* iPhone X 之前   49
* iPhone X           83

判断是否为`iPhone X`  [用 Swift 判断 iPhone X 机型](https://imtx.me/archives/2374.html)
```
extension UIDevice {
    /// 是否是iPhone X
    ///
    /// - Returns: iPhone X
    public func isiPhoneX() -> Bool {
        if UIScreen.main.bounds.height == 812 {
            return true
        }
        return false
    }
}
```
因为自己项目是OC 和 Swift 混编, 所以增加了一个宏
```
#define TABBAR_HEIGHT ([UIDevice currentDevice].isiPhoneX ? 83 : 49)
```

### StatusBar 高度
正常情况下  `statusBar` 高度为22, 但是在`iPhone X` 或 打开热点, 并被连接或后台使用定位等情况下  `statusBar`高度会改变, 所以最好使用系统方法获取`statusBar`高度
```
CGRectGetHeight([UIApplication sharedApplication].statusBarFrame)
```
### 参考文章
[官方文档iPhone X](https://developer.apple.com/ios/human-interface-guidelines/overview/iphone-x/)
[用 Swift 判断 iPhone X 机型](https://imtx.me/archives/2374.html)

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


