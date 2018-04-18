---
layout: post
title:  "NSDate, NSDateFormatter, NSTimeZone, NSCalendar"
date:   2018-04-16 15:29:31 +0800
categories: 开发笔记
---
![](http://yuqiangcoder.com/assets/postImages/ios/201804/NSDateformatter1.png)

### [NSDate](https://developer.apple.com/documentation/foundation/nsdate)
> NSDate对象封装了一个单独的时间点，独立于任何特定的日历系统或时区。Date对象是不可变的，表示相对于绝对引用日期的不变时间间隔(2001年1月1日00:00:00 UTC)

1. 使用断点调试查看 [NSDate date] 显示的是已`UTC`[世界标准时间](https://baike.baidu.com/item/%E5%8D%8F%E8%B0%83%E4%B8%96%E7%95%8C%E6%97%B6/787659?fr=aladdin&fromid=5899996&fromtitle=UTC)的日期时间.
2. 使用输出到控制台的方式 `NSLog(@"%@", [NSDate date]);` 显示的是格式化后的日期时间.


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

