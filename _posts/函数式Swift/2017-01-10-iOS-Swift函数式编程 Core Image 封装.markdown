---
layout: post
title:  "iOS-Swift函数式编程 Core Image 封装"
date:   2017-01-10 10:17:31 +0800
categories: 函数式Swift
---
![](http://yuqiangcoder.com/assets/postImages/ios/201701/2.jpg)

# 说明
> 本文内容均出自[函数式 Swift](https://store.objccn.io/products/functional-swift/)一书, 此处整理仅仅是为了自己日后方便查看, 需要深入研究的话, 可以点进去购买, 支持[原作者](https://store.objccn.io/products/functional-swift/)
本书由 [王巍--新浪微博](http://weibo.com/onevcat?is_hot=1)大神翻译
[OneV's Den 个人博客](https://onevcat.com/#blog) 

# Core Image 简介
> `Core Image`是一个强大的图像处理框架, 但是它的API 略显笨拙.
`Core Image`的API是弱类型----通过键值编码`KVC`来配置图像滤镜`filter`.
在使用参数的类型或名字时, 我们都使用字符串来进行表示, 这十分容易出错, 极有可能导致运行时错误. 
我们开发新的API 将会利用*类型*来避免这些原因导致的运行时错误, 最终我们将得到一组类型安全且高度模块化的API.

# 滤镜类型
> `CIFilter` 是`Core Image`中的核心类之一, 用于创建图像滤镜. 当实例化一个`CIFilter`对象时, 几乎总是通过`kCIInputImageKey`键提供输入图像, 再通过`kCIOutputImageKey` 键取回处理后的图像, 取回的结果可以作为下一个滤镜的输入值.

我们会尝试封装这些键值对的具体细节, 从而呈现给用户一个强类型的API. 我们将Filter定义为一个函数, 该函数接受一个图像作为参数,并返回一个新的图像
```
typealias Filter = (CIImage) -> CIImage
```

# 构建滤镜
> 我们已经定义了`Filter`类型, 接着就可以开始定义函数来构建特定的滤镜了.  这些函在接受特定滤镜所需要的参数之后, 构造并返回一个`Filter`类型的值, 他们的具体形态大概都是这样的: 
```
func myFilter(/* parameters */) -> Filter
```

### 1. 模糊滤镜
> 高斯模糊滤镜--只需要模糊半径这一个参数

```
/// 高斯模糊滤镜
///
/// - Parameter radius: 模糊半径
/// - Returns: 新图像
func blur(radius: Double) -> Filter {
    return { image in
        let parameters = [
            kCIInputRadiusKey: radius,
            kCIInputImageKey: image
            ] as [String : Any]
        guard let filter = CIFilter(name: "CIGaussianBlur", withInputParameters: parameters) else {fatalError()}
        guard let outputImage =  filter.outputImage else {fatalError()}
        return outputImage
    }
}
```

一切就是这么简单, `blur`函数返回一个新函数, 新函数接受一个`CIImage`类型的`image`, 并返回一个新图像(`filter.outputImage`), 因此blur函数满足我们之前定义的
`CIImage->CIImage`, 也就是`Filter`类型.

### 2. 颜色层叠
> 现在我们来定义一个能够在图像上覆盖纯色叠层的滤镜. Core Image默认不包含这样一个滤镜, 但是我们完全可以用已经存在的滤镜来组成它. 
我们将使用的两个基础组件: 颜色生成滤镜(`CIConstantColorGenerator`) 和 图像覆盖合成滤镜(`CISourceOverCompositing`).

* 生成固定颜色的滤镜
```
/// 生成固定颜色的滤镜
///
/// - Parameter color: 颜色
/// - Returns: 新图像
func colorGenerator(color: UIColor) -> Filter {
    return { _ in
        
        let c = CIColor(color: color)
        let parameters = [kCIInputColorKey: c]
        guard let filter = CIFilter(name: "CIConstantColorGenerator", withInputParameters: parameters) else {fatalError()}
        guard let outputImage = filter.outputImage else {fatalError()}
        return outputImage
    }
}
```

* 合成滤镜
```
/// 合成滤镜
///
/// - Parameter overlay: 输出图像
/// - Returns: 新图像
func compositeSourceOver(overlay: CIImage) -> Filter {
    return { image in
        let parameters = [
            kCIInputBackgroundImageKey: image,
            kCIInputImageKey: overlay
        ]
        guard let filter = CIFilter(name: "CISourceOverCompositing", withInputParameters: parameters) else {fatalError()}
        guard let outputImage = filter.outputImage else {fatalError()}
        let cropRect = image.extent
        return outputImage.cropping(to: cropRect)
    }
}
```

* 通过两个滤镜来创建颜色叠层滤镜
```
/// 颜色叠层滤镜
///
/// - Parameter color: 颜色
/// - Returns: 新图像
func colorOverlay(color: UIColor) -> Filter {
    return { image in
        let overlay = colorGenerator(color: color)(image)
        return compositeSourceOver(overlay: overlay)(image)
    }
}
```
我们再次返回了一个接受图像作为参数的函数. `colorOverlay`函数首先调用了`colorGenerator`滤镜, `colorGenerator`滤镜需要一个`color`作为参数, 然后返回一个新的滤镜, 因此代码片段`colorGenerator(color: color)`是`Filter`类型. 而`Filter`类型本身就是一个从`CIImage`到`CIImage`的函数; 因此我们可以向`colorGenerator(color: color)`函数传递一个`CIImage`类型的参数, 最终我们能够得到一个`CIImage`类型的新叠层. 这就是我们在定义`Overlay`过程中所发生的全部事情, 可大致概括为----首先使用`colorGenerator`函数创建一个滤镜, 接着向这个滤镜传递一个`image`参数来创建新图像. 与之类似, 返回值`compositeSourceOver(overlay: overlay)(image)`由一个通过`compositeSourceOver(overlay: overlay)`函数构建的滤镜和随即被作为参数的`image`组成.

### 3.组合滤镜
> 到现在为止, 我们已经定义了模糊滤镜和颜色叠层滤镜, 可以把他们组合在一起使用: 首先将图像模糊, 然后再覆盖一层红色叠层.

让我们载入一张图片试试看:
```
let imageView = UIImageView(frame: view.bounds)
 view.addSubview(imageView)
        
let url = URL(string: "https://raw.githubusercontent.com/Alamofire/Alamofire/assets/alamofire.png")
let image = CIImage(contentsOf: url!)
```

> 现在我们可以链式地将两个滤镜应用到载入的图像上

```

 let blurImage = blur(radius: 10)(image!)
 let overlayColor = UIColor.red.withAlphaComponent(0.2)
 let overlayImage = colorOverlay(color: overlayColor)(blurImage!)
```

### 4.复合函数
> 当然我们可以将上面代码里的两个调用滤镜的表达式简单何为一体:

```
imageView.image = UIImage(ciImage: colorOverlay(color: UIColor.red.withAlphaComponent(0.4))(blur(radius: 5)(image!)))
```
> 然而, 由于括号错综复杂, 这些代码很快失去了可读性, 更好的解决方式是自定义一个运算符来组合滤镜. 为了定义该运算符, 首先我们要定义一个用于组合滤镜的函数: 

```
/// 复合滤镜
///
/// - Parameters:
///   - filter1: 滤镜1
///   - filter2: 滤镜2
/// - Returns: 新图像
func composeFilters(filter1: @escaping Filter, filter2: @escaping Filter) -> Filter {
    return { image in
        filter2(filter1(image))
    }
}
```
`composeFilters`函数接收两个`Filter`类型的参数, 并返回一个新定义的滤镜. 这个符合滤镜接受`CIImage`类型的图像参数, 然后将该参数传递给`filter1`, 取得返回值之后再传递给`filter2`. 我们可以使用符合函数来定义符合滤镜, 像下面这样: 
```
 imageView.image = UIImage(ciImage: composeFilters(filter1: colorOverlay(color: overlayColor), filter2: blur(radius: 5))(image!))
```

> 为了让代码更具可读性, 我们可以再进一步, 为组合滤镜引入运算符. 诚然, 随意自定义运算符, 并不一定对提升代码可读性有帮助. 不过, 在图像处理库中, 滤镜的组合是一个反复被讨论的问题, 所以引入运算符极有意义: 

```
/*
 运算符重载: 中置      前置      后置
          infix    prefix   postfix
 组合赋值运算符: assignment
 自定义运算符:
        全局域使用 operator 关键字声明
 自定义中置运算符的优先级和结合性:
        结合性(associativity) 取值有: left right none
        优先级(precedence)  默认为100
Operator should no longer be declared with body; use a precedence group instead
 */
infix operator >>> : ComposeFilter
precedencegroup ComposeFilter {
    associativity: left             // 左结合
    higherThan: AdditionPrecedence  // 优先级高于加法类型
    lowerThan: MultiplicationPrecedence // 优先级低于减法类型
}
func >>> (filter1: @escaping Filter, filter2: @escaping Filter) -> Filter {
    return { image in
        filter2(filter1(image))
    }
}
```
与之前使用`composeFilters`的方法类似, 我们现在可以使用`>>>`运算符达到目的:
```
imageView.image = UIImage(ciImage: (blur(radius: 5) >>> colorOverlay(color: UIColor.blue.withAlphaComponent(0.4)))(image!))
```
由于已经定义的运算符`>>>`是左结合(`associativity: left`), 就像`Unix`的管道一样, 滤镜将以从左到右的顺序被应用到图像上.

# 总结
> Swift有着合适的语言特性来适配函数式的编程方法.
学习用函数式的方式思考并不容易. 它挑战了我们既有的熟练解决问题的方式.
神奇的Swift~~~😃

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


