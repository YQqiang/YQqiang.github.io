---
layout: post
title:  "Tips"
date:   2019-09-02 15:29:31 +0800
categories: 开发笔记
---

### tableViewHeader
`self.tableView.tableHeaderView` 使用自动布局时会出现约束警告

解决方案： 包一层使用`Frame`布局的父视图

```obj-c
- (void)updateTableViewHeader {
    CGSize size = [self.europeTitleView systemLayoutSizeFittingSize:CGSizeMake(SGConstantUI.screenW, 0) withHorizontalFittingPriority:UILayoutPriorityRequired verticalFittingPriority:UILayoutPriorityFittingSizeLevel];
    UIView *headerView = [[UIView alloc] init];
    headerView.backgroundColor = UIColor.whiteColor;
    [headerView addSubview:self.europeTitleView];
    headerView.frame = CGRectMake(0, 0, size.width, size.height);
    self.europeTitleView.frame = headerView.bounds;
    self.tableView.tableHeaderView = headerView;
}
```

### stackView 设置边距

```obj-c
// 设置 stackView 边距
self.stackView.layoutMarginsRelativeArrangement = YES;
self.stackView.layoutMargins = UIEdgeInsetsMake(0, 16, 0, 16);
```

### copy 和 strong 修饰 NSSting 属性的区别

1. 当赋值的类型是 `NSString` 时，无区别，都是浅拷贝（引用拷贝）
2. 当赋值的类型是 `NSMutableString`时，有区别；
    * 使用 `copy` 修饰，会把字符串内容拷贝（深拷贝）；更改源字符串时，属性值**不会改变**。
    * 使用 `strong` 修饰，只是引用拷贝（浅拷贝）；更改源字符串时，属性值**也会改变**。

### UISearchBar in UINavigationBar

```
self.navigationItem.titleView = searchBar;
```

在`iOS13`系统，当导航栏中放入搜索框时，导航栏高度会由 `44` 增加到 `56`.


可以通过把`searchBar`包一层`UIview`解决

```
UIView *wrapView = [[UIView alloc] initWithFrame:self.frame];
[wrapView addSubview:searchBar];
self.navigationItem.titleView = wrapView;
```

但在`iOS11`系统，`wrapView`的宽度为`0`，可通过自定义`WrapView`，重写`intrinsicContentSize`方法解决

```
@interface WrapNavTitleView : UIView

@end

@implementation WrapNavTitleView

- (CGSize)intrinsicContentSize {
    return UILayoutFittingExpandedSize;
}

@end
```

### 命令行语法格式

> 命令 <必选参数1|必选参数2> [-option {必选参数1|必选参数2|必选参数3}] [可选参数…] {(默认参数)|参数|参数}

符号含义如下：

* 尖括号< >：必选参数，实际使用时应将其替换为所需要的参数

* 大括号{ }：必选参数，内部使用，包含此处允许使用的参数

* 方括号[ ]：可选参数，在命令中根据需要加以取舍

* 小括号( )：指明参数的默认值，只用于{ }中

* 竖线|：用于分隔多个互斥参数，含义为“或”，使用时只能选择一个。

* 省略号…：任意多个参数

例：

```
git pull [options] [<repository> [<refspec>…]]

git push [--all | --mirror | --tags] [--follow-tags] [--atomic] [-n | --dry-run] [--receive-pack=<git-receive-pack>]
       [--repo=<repository>] [-f | --force] [-d | --delete] [--prune] [-v | --verbose]
       [-u | --set-upstream] [--push-option=<string>]
       [--[no-]signed|--sign=(true|false|if-asked)]
       [--force-with-lease[=<refname>[:<expect>]]]
       [--no-verify] [<repository> [<refspec>…]]
```

### URL安全的Base64编码

[wikipedia-Base64](https://zh.wikipedia.org/wiki/Base64)

> `Base64` 编码可用于在 `HTTP` 环境下传递较长的标识信息。例如，在Java持久化系统Hibernate中，就采用了 `Base64` 来将一个较长的唯一标识符（一般为 `128-bit` 的 `UUID` ）编码为一个字符串，用作 `HTTP` 表单和 `HTTP GET URL` 中的参数。在其他应用程序中，也常常需要把二进制数据编码为适合放在 `URL`（包括隐藏表单域）中的形式。此时，采用 `Base64` 编码不仅比较简短，同时也具有不可读性，即所编码的数据不会被人用肉眼所直接看到。

> 然而，标准的 `Base64` 并不适合直接放在 `URL` 里传输，因为 `URL` 编码器会把标准 `Base64`中的 `/` 和 `+` 字符变为形如 `%XX` 的形式，而这些 `%` 号在存入数据库时还需要再进行转换，因为 `ANSI SQL` 中已将 `%` 号用作通配符。

> 为解决此问题，可采用一种用于 `URL` 的改进 `Base64` 编码，它不在末尾填充=号，并将标准 `Base64` 中的 `+` 和 `/` 分别改成了 `-` 和 `_` ，这样就免去了在 `URL` 编解码和数据库存储时所要作的转换，避免了编码信息长度在此过程中的增加，并统一了数据库、表单等处对象标识符的格式。

```Objective-C
+ (NSString *)base64URLSafeEncode:(NSData *)data {
    data = [data base64EncodedDataWithOptions:0];
    NSString *str = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
    str = [str stringByReplacingOccurrencesOfString:@"+" withString:@"-"];
    str = [str stringByReplacingOccurrencesOfString:@"/" withString:@"_"];
    return str;
}

+ (NSData *)base64URLSafeDecode:(NSString *)str {
    str = [str stringByReplacingOccurrencesOfString:@"-" withString:@"+"];
    str = [str stringByReplacingOccurrencesOfString:@"_" withString:@"/"];
    NSData *data = [[NSData alloc] initWithBase64EncodedString:str options:NSDataBase64DecodingIgnoreUnknownCharacters];
    return data;
}
```

### NSInvocation

使用 `NSInvocation` 获取对象类型的返回值时，使用默认的 `__strong` 修饰符修饰变量会引发崩溃；

解决方案：可使用 `__weak` 或 `__autoreleasing` 修饰符修饰变量。

```Objective-C
SEL selector = NSSelectorFromString(@"collectionView");
if ([self.pageContentView respondsToSelector:selector]) {
   NSMethodSignature *signature = [self.pageContentView methodSignatureForSelector:selector];
   NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:signature];
   [invocation setSelector:selector];
   [invocation setTarget:self.pageContentView];
   UICollectionView __autoreleasing *collection;
   [invocation invoke];
   [invocation getReturnValue:&collection];
   if (collection) {
       collection.allowsSelection = NO;
   }
}
```

### popToViewController

用 `popToViewController` 返回到指定控制器时，在当前控制器的 `viewWillAppear` 方法中无法获取 `self.navigationController` 为 `nil` 

解决方案: 更改为 使用 `popViewControllerAnimated`

```Objective-C
NSMutableArray *viewControllers = [self.navigationController.viewControllers mutableCopy];
NSInteger indexCount = viewControllers.count;
if (indexCount > 2 && [viewControllers[indexCount - 2] isKindOfClass:NSClassFromString(@"DemoViewController")]) {
    [viewControllers removeObjectAtIndex:indexCount - 2];
    self.navigationController.viewControllers = [viewControllers copy];
}
[self.navigationController popViewControllerAnimated:YES];
```

### NSProxy
`NSObject` 和 `NSProxy` 都是 `Foundation` 框架中的基类，并且都实现了 `<NSObject>` 这个接口。

通过继承自 `NSObject` 的代理类是不会自动转发 `respondsToSelector:` 和 `isKindOfClass:` 这两个方法的, 而继承自 `NSProxy` 的代理类却是可以的。

`NSObject` 的所有 `Category` 中定义的方法也无法完成转发。


### MAC 密码位数限制去除
macOS Mojave 10.14 以后就要求密码至少要设置四位数，在终端使用 `pwpolicy -clearaccountpolicies` 命令可去除此限制

### IaaS, PaaS, SaaS
IaaS：基础设施服务，Infrastructure-as-a-service
PaaS：平台服务，Platform-as-a-service
SaaS：软件服务，Software-as-a-service

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

