---
layout: post
title:  "Mapping Dictionary keys"
date:   2019-12-06 15:29:31 +0800
categories: Swift
---

![](http://yuqiangcoder.com/assets/postImages/ios/201912/map_dictionary_key_title.jpg)
该图片由<a href="https://pixabay.com/zh/users/markusspiske-670330/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1730939">Markus Spiske</a>在<a href="https://pixabay.com/zh/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1730939">Pixabay</a>上发布

## 问题
在 `OC` 和 `Swift` 混编赋值函数参数时，出现类型不匹配的问题：

`OC`中的方法：

```objc
+ (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString*, id> *)options;
```

其中`options` 的类型为 `NSDictionary<NSString*, id>`；
桥接到 `Swift` 中时，`options` 的类型为 `[String: Any]`。

`Swift`中的方法：

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool
```

其中 `options` 的类型为 `[UIApplication.OpenURLOptionsKey : Any]`

因为 `swift` 是强类型语言，因此无法直接把 `[UIApplication.OpenURLOptionsKey : Any]` 类型的值赋给 `[String: Any]` 类型

类型不一致时赋值，编译器会报错

![](http://yuqiangcoder.com/assets/postImages/ios/201912/different_type_error.png)

## 解决
需要把 `[UIApplication.OpenURLOptionsKey : Any]` 类型 转化为 `[String: Any]` 类型

`UIApplication.OpenURLOptionsKey` 遵循了 `RawRepresentable` 协议，所以通过 `key.rawValue` 即可转换为 `String` 类型

简单粗暴的方式 `for in`

```
var transOptions = [String: Any]()
for x in options {
    transOptions[x.key.rawValue] = x.value
}
```

呃。。。不够优雅，使用 `reduce` 函数简化

```swift
let transOptions = options.reduce(into: [:]) { (result, x) in
    result[x.key.rawValue] = x.value
}
```

呃。。。不够通用，遇到 `NSAttributedString.Key`、`NSNotification.Name` 等类型 或者 `[Int: Any]` 转换 `[String: Any]` 等 `key` 需要转换的时候，需要写重复代码。
泛型封装

```swift
extension Dictionary {
    func compactMapKeys<T>(_ transform: ((Key) throws -> T?)) rethrows -> Dictionary<T, Value> {
       return try self.reduce(into: [T: Value](), { (result, x) in
           if let key = try transform(x.key) {
               result[key] = x.value
           }
       })
    }
}

```

使用方式如下

```swift
let transOptions = options.compactMapKeys {$0.rawValue}

```

## 参考
[swift.org](https://forums.swift.org/t/mapping-dictionary-keys/15342/2)


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

