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



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

