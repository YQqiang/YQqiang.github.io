---
layout: post
title:  "UISearchBar 闪现背景色解决方案"
date:   2017-01-22 10:17:31 +0800
categories: 项目维护
---
![](http://yuqiangcoder.com/assets/postImages/ios/201701/6.jpg)

# 说明
>  此处记录项目中的一个bug解决方案, 希望可以帮助遇到相同问题的小伙伴.
UISearchBar 放在导航栏, 当pop控制器时, searchBar会闪现背景色.

# bug 演示

![searchBar 闪现背景色bug演示.gif](http://yuqiangcoder.com/assets/postImages/ios/201701/7.gif)

# 寻找bug
> UISearchbar 随着iOS的版本升级也在不断的发生着变化
我们可以使用UIView的`recursiveDescription`方法 查看searchBar的视图层次结构, 断点调试: `po [××× recursiveDescription]`
此处测试版本: 模拟器 iOS10.2(14C89)

## 调试结果
```
(lldb) po [searchBar recursiveDescription]
<DopSearchBar: 0x7fd36ed89850; baseClass = UISearchBar; frame = (0 0; 0 0); text = ''; tintColor = UIExtendedGrayColorSpace 0 0; gestureRecognizers = <NSArray: 0x6080004551b0>; layer = <CALayer: 0x60800063b7a0>>
   | <UIView: 0x7fd36efb1b90; frame = (0 0; 0 0); clipsToBounds = YES; autoresize = W+H; layer = <CALayer: 0x608000639cc0>>
   |    | <UISearchBarBackground: 0x7fd36ee2fce0; frame = (0 0; 0 0); userInteractionEnabled = NO; layer = <CALayer: 0x60000042e040>> - (null)
   |    | <UISearchBarTextField: 0x7fd36ed8e950; frame = (0 0; 0 0); text = ''; opaque = NO; tintColor = UIExtendedSRGBColorSpace 0.215686 0.709804 0.988235 1; layer = <CALayer: 0x60000042bec0>>
   |    |    | <UITextFieldBorderView: 0x7fd36ec48520; frame = (0 0; 0 0); clipsToBounds = YES; opaque = NO; layer = <CALayer: 0x60000042e1e0>>
```
ps: DopSearchBar 是子类化的searchBar

searchBar 层级结构如下图
![searchBar 层次结构.png](http://yuqiangcoder.com/assets/postImages/ios/201701/9.png)



# 修复bug
> 我们只需要移除 `UISearchBarBackground`即可

代码如下, 代码中的`self`为子类化的`searchBar`
```
for (UIView *subV in self.subviews) {
        if (subV.subviews.count > 0 && [subV.subviews.firstObject isKindOfClass:NSClassFromString(@"UISearchBarBackground")]) {
            [subV.subviews.firstObject removeFromSuperview];
            break;
        }
    }
```

# 修复bug后的效果

![fix bug.gif](http://yuqiangcoder.com/assets/postImages/ios/201701/8.gif)

# 参考文章
[UISearchbar去除背景色的方法，适合iOS5/6/7/8.0beta](http://blog.csdn.net/forestml2008/article/details/32914915)

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


