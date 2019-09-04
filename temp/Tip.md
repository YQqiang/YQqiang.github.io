---
layout: post
title:  "Tips"
date:   2019-09-02 15:29:31 +0800
categories: 开发笔记
---

### tableViewHeader
`self.tableView.tableHeaderView` 使用自动布局时会出现约束警告

解决方案： 包一层使用`Frame`布局的父视图

```OC
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

```
// 设置 stackView 边距
self.stackView.layoutMarginsRelativeArrangement = YES;
self.stackView.layoutMargins = UIEdgeInsetsMake(0, 16, 0, 16);
```


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

