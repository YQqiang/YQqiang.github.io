# Swift Compare the values of the properties

![](http://yuqiangcoder.com/assets/postImages/ios/201803/2.jpg)

### 问题 <a id="&#x95EE;&#x9898;"></a>

在做表单填写/编辑的功能开发时, 总会遇到类似这样的需求: 当用户正在编辑时,点击了返回按钮, 这时就需要代码判断当前用户有没有更改过表单内容, 如果更改了则需要提示用户当前编辑内容未保存, 是否确定返回; 如果未更改则可以返回上个页面.

在iOS开发中, 这些页面的数据通常会保存在`Model`里, 如果实现上述的需求就需要对比`Model`中的属性值是否发生了改变.

### 功能实现 <a id="&#x529F;&#x80FD;&#x5B9E;&#x73B0;"></a>

为了能够复用代码, 我把上述功能封装成了一个协议, 并且自定义了一个运算符`=*=`, 能够轻松的对比两个同类型的`Model`的属性值是否全部相同.

### 使用步骤 <a id="&#x4F7F;&#x7528;&#x6B65;&#x9AA4;"></a>

1. 遵循协议\(`EquatableProperty`\)
2. 使用运算符\(`=*=`\)进行比较

### 举个例子🌰 <a id="&#x4E3E;&#x4E2A;&#x4F8B;&#x5B50;"></a>

```text
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

### 支持类型如下 <a id="&#x652F;&#x6301;&#x7C7B;&#x578B;&#x5982;&#x4E0B;"></a>

* `class`
* `struct`
* `Int`
* `Bool`
* `Double`
* `Float`
* `String`
* `NSNumber`
* `[Int]`
* `[Bool]`
* `[Double]`
* `[Float]`
* `[String]`
* `[NSNumber]`
* `[String: Int]`
* `[String: Bool]`
* `[String: Double]`
* `[String: Float]`
* `[String: String]`
* `[String: NSNumber]`
* `以上类型的所有可选型`

### 源码地址 <a id="&#x6E90;&#x7801;&#x5730;&#x5740;"></a>

实现源码[EquatableProperty](https://github.com/YQqiang/EquatableProperty)

## 联系我： <a id="&#x8054;&#x7CFB;&#x6211;"></a>

* 博客: http://yuqiangcoder.com/
* 邮箱: yuqiang.coder@gmail.com

