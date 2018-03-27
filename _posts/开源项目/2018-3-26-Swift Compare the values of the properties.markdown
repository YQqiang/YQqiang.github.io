---
layout: post
title:  "Swift Compare the values of the properties"
date:   2018-03-26 15:37:31 +0800
categories: 开源项目
---
![](http://yuqiangcoder.com/assets/postImages/ios/201803/2.jpg)

### 问题
在做表单填写/编辑的功能开发时, 总会遇到类似这样的需求: 当用户正在编辑时,点击了返回按钮, 这时就需要代码判断当前用户有没有更改过表单内容, 如果更改了则需要提示用户当前编辑内容未保存, 是否确定返回; 如果未更改则可以返回上个页面.

在iOS开发中, 这些页面的数据通常会保存在`Model`里, 如果实现上述的需求就需要对比`Model`中的属性值是否发生了改变.

### 功能实现
为了能够复用代码, 我把上述功能封装成了一个协议, 并且自定义了一个运算符`=*=`, 能够轻松的对比两个同类型的`Model`的属性值是否全部相同.

### 使用步骤
1. 遵循协议(`EquatableProperty`)
2. 使用运算符(`=*=`)进行比较

### 举个例子🌰
```
class SuperClassStr: EquatableProperty {
    var propertyStr: String = ""
    var propertyStrOpt: String?
    
    var subClass: SubClass!
}

class SubClass: EquatableProperty {
    var propertyInt: Int = 0
    var propertyIntOpt: Int?
}

let strClass1 = SuperClassStr()
let strClass2 = SuperClassStr()
strClass1.propertyStr = "test123"
strClass2.propertyStr = "test123"
   
let subClass1 = SubClass()
subClass1.propertyInt = 1
let subClass2 = SubClass()
subClass2.propertyInt = 15
   
strClass1.subClass = subClass1
strClass2.subClass = subClass2

let isEqual = strClass1 =*= strClass2
print("isEqual = \(isEqual)")

```

> 注: 如果实例对象中包含的是其他类类型的属性, 则只需要该类也遵循`EquatableProperty`协议, 即可自动比较该类中的属性值.

### 支持类型如下
- [x] `class`
- [x] `struct`
- [x] `Int`
- [x] `Bool`
- [x] `Double`
- [x] `Float`
- [x] `String`
- [x] `NSNumber`
- [x] `[Int]`
- [x] `[Bool]`
- [x] `[Double]`
- [x] `[Float]`
- [x] `[String]`
- [x] `[NSNumber]`
- [x] `[String: Int]`
- [x] `[String: Bool]`
- [x] `[String: Double]`
- [x] `[String: Float]`
- [x] `[String: String]`
- [x] `[String: NSNumber]`
- [x] `以上类型的所有可选型`

### 源码地址
实现源码[EquatableProperty](https://github.com/YQqiang/EquatableProperty)


## 联系我：
- 博客: http://yuqiangcoder.com/
- 邮箱: yuqiang.coder@gmail.com

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


