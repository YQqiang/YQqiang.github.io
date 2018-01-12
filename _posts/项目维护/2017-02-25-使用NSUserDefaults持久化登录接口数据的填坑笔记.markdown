---
layout: post
title:  "使用NSUserDefaults持久化登录接口数据的填坑笔记"
date:   2017-02-25 10:17:31 +0800
categories: 项目维护
---
![](http://yuqiangcoder.com/assets/postImages/ios/201702/14.jpg)

> 项目中一直是使用`NSUserDefaults`把登录接口获取到的用户信息, 保存在本地的`plist`文件. 一切都是那么的Easy, 世界是多么的美好. 直到今天(2017-02-25), 切换服务器(我们APP支持自定义服务器)的时候再登录, 直接闪退, ***what fuck !!!!!! ***

# 寻找闪退原因

> *** 'Attempt to insert non-property list object ***
这是控制台给的闪退原因

1. 断点调试, 发现崩在了 `[[NSUserDefaults standardUserDefaults] setValuesForKeysWithDictionary:dic];` 上, 然后就是对比之前和现在的接口数据有什么不一样. 发现接口多返回了一个字段`sysUser`, 该字段是个字典类型, 里面存储了好多个键值对.

  难道是`NSUserDefaults`不支持字典嵌套字典的格式, 什么鬼????

2. Google 搜索, 基本上都是说`NSUserDefaults`不支持存储自定义的类型, 需要转为`data` 类型才能存储. 但是在这里我是直接把接口的数据存储的, 根本不存在自定义的类型啊?

3. 仔细看了一下接口数据, 发现接口数据有`"<null>"`, 我靠, `NSUserDefaults`解析不了这种类型啊. 

# 解决方案
既然知道了原因所在, 那就对症下药吧!!!

1. 使用`MJExtension` 把登录接口的数据转换为模型`LoginModel`
```
LoginModel *account = [LoginModel mj_objectWithKeyValues:result];
```

2. 继续使用`MJExtension` 把`account` 转为字典, 这样就过滤掉`<null>`
```
NSDictionary *dic = [account mj_keyValues];
```
3. 最后再使用`NSUserDefaults` 持久化数据
```
[[NSUserDefaults standardUserDefaults] setValuesForKeysWithDictionary:dic];
```

# 写在最后
> 填完坑, 瞬间感觉, 天还很蓝, 世界还很美好😜

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


