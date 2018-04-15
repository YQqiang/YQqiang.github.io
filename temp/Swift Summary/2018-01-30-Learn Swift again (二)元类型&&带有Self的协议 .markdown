---
layout: post
title:  "Learn Swift again (一) 特性"
date:   2018-01-30 15:29:31 +0800
categories: Swift Summary
---
![](http://yuqiangcoder.com/assets/postImages/ios/201801/1.png)

### 元类型
> 元类型是指类型的类型, 包括类类型, 结构体类型, 枚举类型和协议类型.
> 类, 结构体或枚举类型的元类型是相应的类型名紧跟`.Type`. 
> 协议类型的元类型--并不是运行时符合该协议的具体类型--而是协议名字紧跟`.Protocol`.

比如: 类`SomeClass`的元类型就是 `SomeClass.Type`.
`SomeProtocol`的元类型就是`SomeProtocol.Protocol`.

你可以使用后缀`self`表达式来获取该类型.比如, `SomeClass.self`返回`SomeClass`本身, 而不是`SomeClass`的一个实例. 同样, `SomeProtocol.self`返回`SomeProtocol`本身, 而不是运行时符合`SomeProtocol`的某个类型的实例.

**可以对类型的实例使用`type(of:)`表达式来获取该实例在运行阶段的类型.**

> 可以使用初始化表达式从某个类型的元类型构造出一个该类型的实例. 对于**类实例**, 被调用的构造器必须使用`required`关键字标记, 或者整个类使用`final`关键字标记.

### 带有Self的协议
1. 协议的两种类型
    * 普通的协议可以被当做类型约束使用, 也可以当做独立的类型使用.
    * 带有关联类型或者`Self`约束的协议特殊一些: 我们不能将它当做独立的类型来使用, 所以像是`let x: Equatable` 这样的写法是不被允许的; 它们只能用作类型约束, 比如 `function f<T: Equatable(x: T)>`.

2. **协议要求**的方法: 在协议定义本身中添加方法的声明
    * 协议要求的方法是动态派发的,
    * 仅定义在扩展中的方法是静态派发的.


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

