---
layout: post
title:  "iOS-时区转换(Objective-C) NSTimeZone"
date:   2017-01-07 10:17:31 +0800
categories: 开发笔记
---
![](http://yuqiangcoder.com/assets/postImages/ios/2017/1.jpg)

## 说明
> 虽然代码很烂, 但还是要记录下来😀
`iOS`中的时间类`NSDate`中存储的时间，都是相对于`GMT`的，我们使用`NSDate`时，会根据App的时区设置返回与时区对应的数据
***所以统一处理成字符串***

## 代码

> 基础方法

*由传入的时区日期转换为目标时区的时间, 并且可以指定原始时区和目标时区是否执行夏令时*

```

/**
 时区转换:传入原始时区的日期,转换到目标时区的日期
 
 @param fromDateStr    原始日期
 @param dateFormateStr 日期格式
 @param fromTimeZone   原始时区
 @param fromIsDst      原始时区是否夏令时
 @param toTimeZone     目标时区
 @param toIsDst        目标时区是否夏令时
 
 @return 目标日期
 */
+ (NSString *)stringFromDate:(NSString *)fromDateStr dateFormate:(NSString *)dateFormateStr fromTimeZone:(NSString *)fromTimeZone fromIsDst:(BOOL)fromIsDst toTimeZone:(NSString *)toTimeZone toIsDst:(BOOL)toIsDst {
    NSRange fromTimeZonRange = [fromTimeZone rangeOfString:@"+"];
    if (fromTimeZonRange.length == 0) {
        fromTimeZonRange = [fromTimeZone rangeOfString:@"-"];
    }
    if (fromTimeZonRange.length == 0) return fromDateStr;
    NSArray *fromMinuteDigitArr = [fromTimeZone componentsSeparatedByString:@"."];
    NSInteger fromMinuteDigit = fromMinuteDigitArr.count == 2 ? [fromMinuteDigitArr.lastObject integerValue] * 60 : 0;
    NSString *fromMinuteDigitStr = [[NSString stringWithFormat:@"%02ld",fromMinuteDigit] substringToIndex:2];
    fromTimeZone = [NSString stringWithFormat:@"%@%02d%@",[fromTimeZone substringToIndex:fromTimeZonRange.location + 1], [[fromTimeZone substringFromIndex:fromTimeZonRange.location + 1] intValue],fromMinuteDigitStr];
    NSRange toTimeZonRange = [toTimeZone rangeOfString:@"+"];
    if (toTimeZonRange.length == 0) {
        toTimeZonRange = [toTimeZone rangeOfString:@"-"];
    }
    if (toTimeZonRange.length == 0) return fromDateStr;
    
    NSArray *toMinuteDigitArr = [toTimeZone componentsSeparatedByString:@"."];
    NSInteger toMinuteDigit = toMinuteDigitArr.count == 2 ? [toMinuteDigitArr.lastObject integerValue] * 60 : 0;
    NSString *toMinuteDigitStr = [[NSString stringWithFormat:@"%02ld",toMinuteDigit] substringToIndex:2];
    toTimeZone = [NSString stringWithFormat:@"%@%02d%@",[toTimeZone substringToIndex:toTimeZonRange.location + 1], [[toTimeZone substringFromIndex:toTimeZonRange.location + 1] intValue],toMinuteDigitStr];
    static NSDateFormatter *dateFormatter = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        dateFormatter = [[NSDateFormatter alloc] init];
    });
    dateFormatter.dateFormat = dateFormateStr;
    //将原始时间转换成原始时区对应的时间
    dateFormatter.timeZone = [NSTimeZone timeZoneWithAbbreviation:fromTimeZone];
    NSDate *fromDate = [dateFormatter dateFromString:fromDateStr];
    
    NSTimeInterval timeINterval = 1 * 60 * 60;
    //如果原始时区执行夏令时,则需要先减少1小时
    if (fromIsDst) {
        fromDate = [NSDate dateWithTimeInterval:-timeINterval sinceDate:fromDate];
    }
    
    //转换成目标时区的时间
    dateFormatter.timeZone = [NSTimeZone timeZoneWithAbbreviation:toTimeZone];
    NSString *toDateStr = [dateFormatter stringFromDate:fromDate];
    NSDate *toDate = [dateFormatter dateFromString:toDateStr];
    
    //如果目标时区执行夏令时,则需要加上1小时
    if (toIsDst) {
        toDate = [NSDate dateWithTimeInterval:timeINterval sinceDate:toDate];
    }
    //目标日期字符串
    toDateStr = [dateFormatter stringFromDate:toDate];
    //如果转换异常,返回原始日期; 转换成功,返回转换后的日期
    return ISNULL(toDateStr) ? fromDateStr : toDateStr;
}
```

> 用户时区和东八区互转
 >> 1. 用户区时间 ----> 东八区时间
2. 东八区时间 ----> 用户区事件

*用户时区日期转换为东八区日期*

```
/**
 用户时区时间 ----> 东8区时间
 
 @param userTime     用户区的时间
 @param dateFormate  转换的时间格式
 @param userTimeZone 用户所在时区
 @param isDst        用户所在时区是否夏令时
 
 @return 对应东8区的时间
 */
+ (NSString *)userTime2GMT8:(NSString *)userTime dateFormate:(NSString *)dateFormate userTimeZone:(NSString *)userTimeZone isDst:(BOOL)isDst {
    return [NSString stringFromDate:userTime dateFormate:dateFormate fromTimeZone:userTimeZone fromIsDst:isDst toTimeZone:@"GMT+8" toIsDst:NO];
}
```

*东八区日期转换为用户时区日期*

```
/**
 东8区时间 ----> 用户区时间
 
 @param gmt8Time     东8区时间
 @param dateFormate  转换的时间格式
 @param userTimeZone 用户区的时区
 @param isDst        用户区是否夏令时
 
 @return 对应用户区的时间
 */
+ (NSString *)GMT8Time2User:(NSString *)gmt8Time dateFormate:(NSString *)dateFormate userTimeZone:(NSString *)userTimeZone isDst:(BOOL)isDst {
    return [NSString stringFromDate:gmt8Time dateFormate:dateFormate fromTimeZone:@"GMT+8" fromIsDst:NO toTimeZone:userTimeZone toIsDst:isDst];
}
```

> 自己在项目中的使用

*用户时区日期(使用手机时间和接口提供的用户时区,及是否夏令时) 转换为东八区日期*

```
/**
 用户时区时间(使用手机时间和接口提供的用户时区,及是否夏令时) ----> 东8区时间
 
 @param self    用户时区时间(手机时间)
 @param dateFormate 转换的时间格式
 
 @return 东8区时间
 */
- (NSString *)userTime2GMT8WithDefaultTimeZoneDateFormate:(NSString *)dateFormate {
    return [NSString userTime2GMT8:self dateFormate:dateFormate userTimeZone:USER_DEFAULT_TIMEZONE isDst:USER_IS_DST];
}
```

*东八区日期转换为用户区日期(使用接口提供的用户时区,及夏令时)*

```
/**
 东8区时间 ----> 用户区时间(使用接口提供的用户时区,及夏令时)
 
 @param self    东8区时间
 @param dateFormate 转换的时间格式
 
 @return 用户时区时间 (使用接口提供的用户时区,及是否夏令时)
 */
- (NSString *)GMT8Time2UserWithDefaultTimeZoneDateFormate:(NSString *)dateFormate {
    return [NSString GMT8Time2User:self dateFormate:dateFormate userTimeZone:USER_DEFAULT_TIMEZONE isDst:USER_IS_DST];
}
```

## 参考文章
[iOS时间那点事--NSTimeZone](https://my.oschina.net/yongbin45/blog/151376)

[iOS时间那点事--NSDateFormatter](https://my.oschina.net/yongbin45/blog/150667)

[iOS 时区问题总结 NSTimeZone](http://www.cnblogs.com/qiutangfengmian/p/5288201.html)

**还有一些文章看过后找不到了, 此处没贴, 深表歉意~~~**

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


