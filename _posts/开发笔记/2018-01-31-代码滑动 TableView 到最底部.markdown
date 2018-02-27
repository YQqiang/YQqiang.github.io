---
layout: post
title:  "代码滑动 TableView 到最底部"
date:   2018-01-31 23:20:31 +0800
categories: 开发笔记
---

### 方案一
最先想到的方案就是使用`scrollToRowAtIndexPath`

```
[self.tableView scrollToRowAtIndexPath:[NSIndexPath indexPathForRow:self.dataSource.count - 1 inSection:0] atScrollPosition:UITableViewScrollPositionBottom animated:YES]
```

经测试发现, 这种方案在数据源过多的情况下会造成卡顿.
在我的项目中表现为数据源为100条时, 卡顿4s左右. **遂使用方案二**.

### 方案二
直接设置 `tableView` 的内容偏移量 `contentOffset`, 然后刷新列表

```
[self.tableView setContentOffset:CGPointMake(0, CGFLOAT_MAX)];
[self.tableView reloadData];
```

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

