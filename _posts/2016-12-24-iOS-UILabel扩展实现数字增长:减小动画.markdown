---
layout: post
title:  "iOS-UILabel扩展实现数字增长/减小动画"
date:   2016-12-24 10:17:31 +0800
categories: jekyll update
---
![](http://upload-images.jianshu.io/upload_images/3538284-4f6bbcf66c1f4c52.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# AnimationUILabel
# 说明
该版本为Swift版本 ,原文在这[OC版本](https://github.com/ScottZg/AnimationNumLabel)
# 效果展示

![数字减小.gif](http://upload-images.jianshu.io/upload_images/3538284-f3f39d16bf4007e2.gif?imageMogr2/auto-orient/strip)

![数字增长.gif](http://upload-images.jianshu.io/upload_images/3538284-e266230237b7c2d1.gif?imageMogr2/auto-orient/strip)

# 主要代码

```
/// 获取时长
    ///
    /// - Parameter num: 数字
    /// - Returns: 返回显示时间
    func getTimeDurationFromNum(num: Double) -> Double {
        if num <= 0 {
            return 0
        } else if num < 1000 {
            return 1
        } else if num < 2000 {
            return num / 1000
        } else {
            return 1
        }
    }
    
    func animation(_ fromNum: Double, toNum: Double, duration: Double) -> Void {
        self.text = String(format: "%0.2f", fromNum)
        let totalCountInt = self.getCountFromNum(num: fabs(toNum - fromNum))
        let totalCount = Double(totalCountInt)
        let delayTime = duration / totalCount
        var mediumNumArr: [String] = [String]()
        for i in 0..<Int(totalCount) {
            if (toNum - fromNum) > 0 {
                mediumNumArr.append(String(format: "%.2f", Double(i) * ((toNum-fromNum)/totalCount)+fromNum))
            } else {
                mediumNumArr.append(String(format: "%.2f", fromNum - Double(i) * ((fromNum - toNum) / totalCount)))
            }
            if i == Int(totalCount) - 1 {
                mediumNumArr.append(String(format: "%.2f", toNum))
            }
        }
        changeLabelTitle(delayTime, mediumArr: &mediumNumArr)
    }
    
    /// 得到分割数目
    ///
    /// - Parameter num: 数字
    /// - Returns: 分割数目
    func getCountFromNum(num: Double) -> Int {
        if num <= 0 {
            return 1
        } else if num < 1000 {
            return 100
        } else {
            return Int(num / 20)
        }
    }
    
    func changeLabelTitle(_ delayTime: Double, mediumArr: inout [String]) {
        if mediumArr.count <= 0 {
        } else {
            self.text = mediumArr.first
            mediumArr.remove(at: 0)
            var temp = mediumArr
            DispatchQueue.main.asyncAfter(deadline: DispatchTime.now() + delayTime, execute: {
                self.changeLabelTitle(delayTime, mediumArr: &temp)
            })
        }
    }
    
    func stopRuning() {
    
    }
```
# 使用方法

```
animationLabel.stopRuning()
animationLabel.animation(start, toNum: end, duration: animationLabel.getTimeDurationFromNum(num: fabs(end)))
```
# demo地址
github地址[AnimationUILabel](https://github.com/YQqiang/AnimationUILabel)
# 写在最后 本人iOS开发菜鸟一枚,不妥之处,还望大神指教~~~~

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


