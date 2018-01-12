---
layout: post
title:  "iOS-UILabel扩展实现数字增长/减小动画"
date:   2016-12-24 10:17:31 +0800
categories: 开发笔记
---
![](http://yuqiangcoder.com/assets/postImages/ios/201612/4.png)

# AnimationUILabel
# 说明
该版本为Swift版本 ,原文在这[OC版本](https://github.com/ScottZg/AnimationNumLabel)
# 效果展示

![数字减小.gif](http://yuqiangcoder.com/assets/postImages/ios/201612/5.gif)

![数字增长.gif](http://yuqiangcoder.com/assets/postImages/ios/201612/6.gif)

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


