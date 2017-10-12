---
layout: post
title:  "iPhone X UI适配小结"
date:   2017-09-26 20:01:31 +0800
categories: 项目维护
---
![](http://upload-images.jianshu.io/upload_images/3538284-27726434142f4634.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 2017-10-07 更新
[苹果官方中文版iPhone X 适配指南](https://developer.apple.com/cn/ios/update-apps-for-iphone-x/)

---

### 启动图片 launchImage
1. 若使用`LaunchScreen.storyboard`, 则自动会拉伸至全屏幕
![使用LaunchScreen.storyboard](http://upload-images.jianshu.io/upload_images/3538284-6ea93171daece234.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 若使用`Launch Images source`
![使用Launch Images source](http://upload-images.jianshu.io/upload_images/3538284-58ee1b49f74e0af4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
需要一张`1125px × 2436px`分辨率的资源文件

![需要的资源文件](http://upload-images.jianshu.io/upload_images/3538284-99a42f3197df59eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![资源文件尺寸](http://upload-images.jianshu.io/upload_images/3538284-cd400296391476ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

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


