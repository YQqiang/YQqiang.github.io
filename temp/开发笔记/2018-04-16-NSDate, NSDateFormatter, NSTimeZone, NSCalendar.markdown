---
layout: post
title:  "NSDate, NSDateFormatter, NSTimeZone, NSCalendar, NSLocale"
date:   2018-04-16 15:29:31 +0800
categories: 开发笔记
---
![](http://yuqiangcoder.com/assets/postImages/ios/201804/NSDateformatter1.png)

### [NSDate](https://developer.apple.com/documentation/foundation/nsdate)
> NSDate对象封装了一个单独的时间点，独立于任何特定的日历系统或时区。Date对象是不可变的，表示相对于绝对引用日期的不变时间间隔(2001年1月1日00:00:00 UTC)

1. 使用断点调试查看 [NSDate date] 显示的是已`UTC`[世界标准时间](https://baike.baidu.com/item/%E5%8D%8F%E8%B0%83%E4%B8%96%E7%95%8C%E6%97%B6/787659?fr=aladdin&fromid=5899996&fromtitle=UTC)的日期时间.
2. 使用输出到控制台的方式 `NSLog(@"%@", [NSDate date]);` 显示的是格式化后的日期时间.

    ![](http://yuqiangcoder.com/assets/postImages/ios/201804/1.png)
    * ① 当前系统时间
    * ② 断点调试显示时间
    * ③ 输出到控制台的时间

### [NSDateFormatter](https://developer.apple.com/documentation/foundation/nsdateformatter)
> 日期和文本字符串之间相互转换的格式化程序.
对于用户可见的日期和时间表示, NSDateFormatter提供了各种本地化的预设置和配置选项.
对于日期和时间的固定格式表示, 可以指定自定义格式字符串

##### [NSTimeZone](https://developer.apple.com/documentation/foundation/nstimezone)
> 时区代表了地缘政治区域的标准时间政策.
时区也可以代表时间的偏离——从格林尼治标准时间(GMT)开始. 例如, 太平洋标准时间的时间偏移距格林尼治标准时间8小时(GMT-8). 您可以使用init(forSecondsFromGMT:)创建时间区域对象, 并传入一个时间偏移量.

PS: 格林尼治标准时间以格林尼治天文台的经线为0 度经线, 将世界分为24 个时区. 为了方便, 在不需要精确到秒的情况下, 通常将`GMT` 和`UTC` 视作等同. 但`UTC` 更加科学更加精确, 它是以原子时为基础, 在时刻上尽量接近世界时的一种时间计量系统.

##### [NSCalendar](https://developer.apple.com/documentation/foundation/nscalendar)
> NSCalendar对象封装了有关定时系统的信息, 其中定义了一年的开始时间, 长度和分区. 它们提供有关日历的信息和对日历计算的支持, 例如确定给定日历单位的范围并将单位添加到给定的绝对时间.
大多数地区使用最广泛使用的公民日历, 即公历. 但也有例外:
在泰国, 一些地方主要使用[佛教日历](https://baike.baidu.com/item/%E4%BD%9B%E5%8E%86/10633813?fr=aladdin)（佛教）.

##### [NSLocale](https://developer.apple.com/documentation/foundation/nslocale)
> 用来格式化和解释关于用户的习惯和偏好的信息.
例如: `NSDateFormatter`类具有一个`locale`属性, 可确保将日期转换为符合用户对日期格式设置的期望的字符串.默认情况下, 此属性表示用户的当前区域设置, 通常是您想要的行为, 但您可以将其设置为另一个区域以获取不同的输出.

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

