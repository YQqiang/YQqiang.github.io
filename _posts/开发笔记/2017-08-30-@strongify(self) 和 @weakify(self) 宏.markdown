---
layout: post
title:  "@strongify(self) 和 @weakify(self) 宏"
date:   2017-08-30 18:45:31 +0800
categories: 开发笔记
---
![](http://yuqiangcoder.com/assets/postImages/ios/201708/3.png)
### 宏展开
首先看一下宏展开的样子
> strongify(self) = autoreleasepool {} _Pragma("clang diagnostic push") _Pragma("clang diagnostic ignored \"-Wshadow\"") __attribute__((objc_ownership(strong))) __typeof__(self) self = self_weak_; _Pragma("clang diagnostic pop")

>  weakify(self) = autoreleasepool {} __attribute__((objc_ownership(weak))) __typeof__(self) self_weak_ = (self);

### 如何打印宏展开内容
> 宏定义中包含两个运算符: # 和 ##

* \# 运算符将一个宏的参数转换为字符串字面量, 即在对它所引用的宏变量通过替换后在其左右各加上一个双引号;
* \## 被称为 连接符, 它是一种预处理运算符, 用来把两个语言符号组合成单个语言符号.

> 打印宏展开内容时, 使用到了`#`运算符

定义展开宏内容的宏
```
#define CONNECT(x)         @#x
#define INPUT(x)           CONNECT(x)
```
> `INPUT(x) ` 通过传入一个宏, 即可返回该宏展开后的内容字符串.

问题: 为何不能直接使用`#define INPUT(x)           @#x`, 必须要使用宏嵌套;
> 首先，C语言的宏是允许嵌套的，其嵌套后，一般的展开规律像函数的参数一样：先展开参数，再分析函数，即由内向外展开。但是，注意：
(1) 当宏中有#运算符时，参数不再被展开；
(2) 当宏中有##运算符时，则先展开函数，再展开里面的参数；
[关于嵌套宏的使用](http://blog.csdn.net/delphiwcdj/article/details/7040247)

## 打印宏展开后的内容
```
    NSString *strongify = INPUT(strongify(self));
    NSLog(@"strongify(self) = %@", strongify);
    
    NSString *weakify = INPUT(weakify(self));
    NSLog(@"weakify(self) = %@", weakify);
```

```
strongify(self) = autoreleasepool {} _Pragma("clang diagnostic push") _Pragma("clang diagnostic ignored \"-Wshadow\"") __attribute__((objc_ownership(strong))) __typeof__(self) self = self_weak_; _Pragma("clang diagnostic pop")

weakify(self) = autoreleasepool {} __attribute__((objc_ownership(weak))) __typeof__(self) self_weak_ = (self);
```

## 浅析strongify(self) 和 weakify(self)
浅析:
> weakify(self) = autoreleasepool {} __attribute__((objc_ownership(weak))) __typeof__(self) self_weak_ = (self);

`@autoreleasepool {} __weak __typeof__(self) self_weak = self;` 转换后的弱引用self前增加一个自动释放池.

打印`#define YQWeakSelf  autoreleasepool {} __weak __typeof__(self) self_weak = self;`
> 结果为:yqWeakSelf(self) = autoreleasepool {} __attribute__((objc_ownership(weak))) __typeof__(self) self_weak = self;

是不是已经很接近`weakify(self)`了😄

同理可以打印`#define YQWeakSelf  autoreleasepool {} __weak __typeof__(self) self_weak = self;`
只不过缺少一个自动释放池和强制忽略警告代码

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

