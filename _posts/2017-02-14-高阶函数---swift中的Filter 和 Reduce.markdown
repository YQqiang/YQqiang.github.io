---
layout: post
title:  "高阶函数---swift中的Filter 和 Reduce"
date:   2017-02-14 10:17:31 +0800
categories: jekyll update
---
![](http://upload-images.jianshu.io/upload_images/3538284-0bc34a563338d07e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 说明
> 本文内容均出自[函数式 Swift](https://store.objccn.io/products/functional-swift/)一书, 此处整理仅仅是为了自己日后方便查看, 需要深入研究的话, 可以点进去购买, 支持[原作者](https://store.objccn.io/products/functional-swift/)
本书由 [王巍--新浪微博](http://weibo.com/onevcat?is_hot=1)大神翻译
[OneV's Den 喵神博客](https://onevcat.com/#blog)

---

> 上一篇写到了[高阶函数---Swift中的泛型介绍(一步步实现map函数)](http://www.jianshu.com/p/b7d2adcedb14)
`map函数`并不是Swift标准数组库中唯一一个使用泛型的函数. 趁着今天有时间, 把`Filter` 和 `Reduce`也整理一下, 方便日后查阅.

# Filter
---
> 假设我们有一个由字符串组成的数组, 代表文件夹的内容: 

```
let exampleFiles = ["README.md", "HelloWorld.swift", "FlappyBird.swift"]
```

> 现在如果我们想要一个包含所有`.swift`文件的数组, 可以很容易通过简单的循环得到: 

```
func getSwiftFiles(_ files: [String]) -> [String] {
        var result: [String] = []
        for file in files {
            if file.hasSuffix(".swift") {
                result.append(file)
            }
        }
        return result
    }
```

> 现在可以使用这个函数来取得`exampleFiles`数组中的`Swift`文件

```
print("\(getSwiftFiles(exampleFiles))")

```

![获取数组中的Swift文件](http://upload-images.jianshu.io/upload_images/3538284-ad28945db49f5cbb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 当然, 我们可以将`getSwiftFiles`函数一般化. 比如, 相比于使用硬编码(hardcoding)的方式筛选扩展名为`.swift`的文件, 传递一个附加的`String`参数进行比对会是更好的方法. 我们接下来可以使用同样的函数去比对`.swift`或`.md`文件. 但是假如我们想查找没有扩展名的所有文件, 或者是名字以字符串`Hello`开头的文件, 那怎么办呢?

---
> 为了进行一个这样的查找, 我们可以定义一个名为`filter`的通用型函数. 就像之前看到的[map](http://www.jianshu.com/p/b7d2adcedb14)那样, `filter`函数接受一个***函数***作为参数. `filter`函数的类型是`Element -> Bool` ------对于数组中的所有元素, 此函数都会判定它是否应该被包含在结果中: 

```
extension Array {
    func filter(includeElement: (Element) -> Bool) -> [Element] {
        var result: [Element] = []
        for x in self where includeElement(x) {
            result.append(x)
        }
        return result
    }
}
```

> 根据`filter` 能很容易地定义`getSwiftFiles`:

```
func getSwiftFiles(_ files: [String]) -> [String] {
        return files.filter(includeElement: { (file) -> Bool in
            file.hasSuffix(".swift")
        })
    }
```

> 就像`map`一样, `Swift`标准库中的数组类型已经有定义好的`filter函数`了. 所以除非是作为练习, 否则并没有必要重写它.

# Reduce
---
> 在定义一个泛型函数来体现一个更常见的模式之前, 我们会先考虑一些相对简单的函数.


> 定义一个计算数组中所有整型值之和的函数非常简单:

```
func sum(_ xs: [Int]) -> Int {
        var result: Int = 0
        for x in xs {
            result += x
        }
        return result
    }
```

> 我们可以向下面这样使用`sum函数`

```
print(sum([1, 2, 3, 4]))
```

![sum结果](http://upload-images.jianshu.io/upload_images/3538284-bcbb4dd871eafc4a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 我们也可以使用类似`sum`中的`for循环`来定义一个 `product函数`, 用于计算所有数组项相乘之积:

```
func product(_ xs: [Int]) -> Int {
        var result: Int = 1
        for x in xs {
            result = x * result
        }
        return result
    }
```

> 同样地, 我们可能想要连接数组中的所有字符串:

```
func concatenate(_ xs: [String]) -> String {
        var result: String = ""
        for x in xs {
            result += x
        }
        return result
    }
```

> 或者说, 我们可以选择连接数组中的所有字符串, 并插入一个单独的首行, 以及在每一项后面追加一个换行符:

```
func prettyPrintArray(_ xs: [String]) -> String {
        var result: String = ""
        for x in xs {
            result = " " + result + x + "\n"
        }
        return result
    }
```

> 这些函数有什么共同特点呢? 他们都将变量`result`初始化为某个值. 随后对输入数组  `xs` 的每一项进行遍历, 最后以某种方式更新结果. 为了定义一个可以体现所需类型的泛型函数, 我们需要对两份信息进行抽象: 赋给 `result`变量的初始值, 和用于在每一次循环中更新`result`的***函数***

> 考虑到这一点, 我们得出了能够匹配此模式的`reduce函数`定义, 如下所示: 

```
extension Array {
    func reduce<T>(initial: T, combine: (T, Element) -> T) -> T {
        var result = initial
        for x in self {
            result = combine(result, x)
        }
        return result
    }
}
```

> 这个函数的泛型类型体现在两个方面: 对于任意的`[Element]`类型的***输入数组***来说, 它会计算一个类型为`T`的返回值. 这么做的前提是, 首先需要一个`T`类型的初始值(赋值给`result`变量), 以及一个用于更新`for循环`中变量值的函数`combine:(T, Element) -> T`. 在一些想`OCaml` 和 `Haskell`一样的函数式语言中, `reduce`函数被称为`fold` 或 `fold_left` 

> 我们可以使用 `reduce`来定义以上函数. 下面是几个例子: 

```
func sumUsingReduce(_ xs: [Int]) -> Int {
        return xs.reduce(0) { (result, x) -> Int in
            return result + x
        }
    }
```

> 除了写一个闭包, 我们也可以将运算符作为最后一个参数. 这使得代码更短, 如下面两个函数所示: 

```
func productUsingReduce(_ xs: [Int]) -> Int {
        return xs.reduce(initial: 1, combine: *)
    }
    
    func concatUsingReduce(_ xs: [String]) -> String {
        return xs.reduce(initial: "", combine: +)
    }
```

> 需要再一次说明, 我们自定义`reduce`仅仅只是为了练习. `Swift`的标准库已经为数组提供了`reduce函数`

---

> 我们可以使用`reduce` 来定义新的泛型函数. 例如, 假设有一个数组, 它的每一项都是数组, 而我们想将他展开为一个单一数组. 可以使用`for循环`编写一个函数:

```
func flatten<T>(_ xss: [[T]]) -> [T] {
        var result: [T] = []
        for xs in xss {
            result += xs
        }
        return result
    }
```

> 然而, 若使用`reduce`则可以想下面这样编写这个函数:

```
func flattenUsingReduce<T>(_ xss: [[T]]) -> [T] {
        return xss.reduce(initial: [], combine: { (result, xs) -> [T] in
            return result + xs
        })
    }
```

> 实际上, 我们甚至可以使用`reduce`重新定义`map` 和 `filter`:

```
func mapUsingReduce<T>(_ transform: (Element) -> T) -> [T] {
        return reduce(initial: [], combine: { (result, x) -> [T] in
            return result + [transform(x)]
        })
    }
    
    func flterUsingReduce(_ includeElement:(Element) -> Bool) -> [Element] {
        return reduce(initial: [], combine: { (result, x) -> [Element] in
            return includeElement(x) ? result + [x] : result
        })
    }
```

> 我们能够使用`reduce`来表示所有这些函数, 这个事实说明了`reduce`能够通过通用的方法来体现一个相当常见的编程模式: 遍历数组并计算结果.

---

> 请务必注意: 尽管通过`reduce`来定义一切是个很有趣的练习, 但是在实践中者往往不是一个什么好主意. 原因在于, 不出意外的话你的代码最终会在运行期间大量复制生成的数组, 换句话说, 它不得不反复分配内存, 释放内存, 以及复制大量内存中的内容.

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


