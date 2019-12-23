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


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

